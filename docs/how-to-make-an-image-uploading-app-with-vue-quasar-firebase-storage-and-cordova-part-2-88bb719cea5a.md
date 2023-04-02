# 如何用 Vue、Quasar、Firebase Storage 和 Cordova 制作一个图片上传应用程序——第二部分

> 原文：<https://javascript.plainenglish.io/how-to-make-an-image-uploading-app-with-vue-quasar-firebase-storage-and-cordova-part-2-88bb719cea5a?source=collection_archive---------2----------------------->

# 我们正在建造的东西

我们将构建一个跨平台的移动应用程序，用于拍摄照片并上传到 firebase。

在 [Part 1](https://medium.com/javascript-in-plain-english/how-to-make-an-image-uploading-app-with-vue-quasar-firebase-storage-and-cordova-part-1-232b68755d0c) 中，我们看到了如何拍摄照片并保存到 Firebase 云存储中。在这篇文章中，我们将通过 web worker 将上传转移到一个单独的线程，并使用 blueimp 库在本地生成一个缩略图，并在上传时显示它。

# 网络工作者

## 什么是网络工作者

> Web Workers 是 Web 内容在后台线程中运行脚本的一种简单方式。工作线程可以在不干扰用户界面的情况下执行任务。([mozilla.org](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers))

因此，我们将使用一个 web worker 将可能阻塞我们的 UI 线程的代码(在我们的例子中是 Firebase 存储代码)卸载到另一个线程。

## 配置工作化加载程序

我们将使用 [workerize-loader](https://github.com/developit/workerize-loader) 来使使用 web workers 变得更容易一些(web workers 接口有点奇怪)。

```
yarn add workerize-loader
```

我们需要添加一些 webpack 配置来告诉 webpack 使用`workerize-loader`。在`quasar.conf.js`的`build`部分添加:

```
extendWebpack(cfg) {
    cfg.module.rules.push({
        test: /\.worker\.js$/,
        loader: 'workerize-loader',
    })
},

chainWebpack (chain, { isServer, isClient }) {
    chain.output.globalObject('self')
}
```

这告诉 webpack 使用`workerize-loader`加载文件`worker.js`。

## 将代码转移到工作线程

让我们添加文件`src/services/worker.js`，并将我们的上传代码移入其中:

```
import cloudStorage from './cloud-storage'

export async function uploadPicture(imageData) {
    let storageId = new Date().getTime().toString();

    let downloadURL = await cloudStorage.uploadBase64(
        imageData,
        storageId
    );

    return {
        storageId,
        downloadURL,
    }
}

export function initFB() {
    cloudStorage.initialize();
}

export async function deletePic(storageId) {
    await cloudStorage.deleteFromStorage(storageId)
}
```

我们还添加了一个`deletePic`调用，稍后我们将在`cloudStorage.js`中看到。

# 添加简单的状态管理

我们希望创建一个 worker 实例，并将其保存在应用程序的状态中。我们还没有状态管理，所以让我们使用一个`store.js`文件添加一个简单的状态管理模式:

```
import worker from "workerize-loader!./worker.js";

var store = {
    debug: true,
    state: {
        pics: [],
        uploading: false
    },
    initWorker() {
        this.state.workerInstance = worker();
        this.state.workerInstance.initFB();
    },
    async loadPictures() {
        if (this.debug) console.log("loadPictures triggered")
        let picsJson = await localStorage.getItem("pics");
        if (!picsJson) this.state.pics = [];
        else this.state.pics = JSON.parse(picsJson);
        this.state.pics = this.state.pics.filter(pic => !pic.uploading)
    },
    async addPic(pic) {
        if (this.debug) console.log("addPic triggered with", pic)

        pic.failed = false;
        this.state.pics.splice(0, 0, pic);
        localStorage.setItem("pics", JSON.stringify(this.state.pics));
    },
    async deletePic(idx) {
        if (this.debug) console.log("deletePic triggered with", idx)

        this.state.pics[idx].uploading = true;

        if (this.state.pics[idx].storageId) {
            await this.state.workerInstance.deletePic(this.state.pics[idx].storageId)
        }
        this.state.pics.splice(idx, 1);
        localStorage.setItem("pics", JSON.stringify(this.state.pics));
    },
    async updatePicUploaded(oldPic, newPic) {
        if (this.debug) console.log("updatePicUploaded triggered with", oldPic, newPic)

        oldPic.uploading = false
        oldPic.url = newPic.downloadURL
        oldPic.storageId = newPic.storageId
        oldPic.width = newPic.width
        oldPic.height = newPic.height

        localStorage.setItem("pics", JSON.stringify(this.state.pics));
    },
    async updatePicFailed(pic) {
        if (this.debug) console.log("updatePicFailed triggered with", pic)

        pic.failed = true
    },
}

export default {
    ...store
}
```

这里发生了一些事情:

*   我们使用前缀`workerize-loader!`导入我们的 worker，它告诉 webpack 使用我们之前配置的加载器。
*   我们将`pics`集合从`Index.vue`移动到了`state`对象。
*   我们公开了初始化 worker 实例的方法`initWorker`。
*   我们在`localStorage` : `addPic`、`loadPictures`和`delete`中添加了一些 CRUD 方法来持久化`pics`集合。
*   `updatePicUploaded`和`updatePicFailed`改变图片的`loading`属性。我们将使用它来显示微调器。

# 生成缩略图

我们将生成一个缩略图(在客户端上)在图像上传时显示。为此，我们将使用 [blueimp](https://www.npmjs.com/package/blueimp-load-image) 库:

```
yarn add blueimp-load-image
```

让我们添加另一个操作图像的服务:`src/services/image-ops.js`:

```
import loadImage from 'blueimp-load-image'

const base64JpegPrefix = "data:image/jpeg;base64,";

function removeBase64Prefix(base64Str) {
    return base64Str.substr(base64Str.indexOf(",") + 1);
}

function addBase64Prefix(imageData) {
    return base64JpegPrefix + imageData
}

async function generateThumbnail(imageData, maxWidth) {
    return new Promise(async resolve => {
        let url = base64JpegPrefix + imageData
        let res = await fetch(url)
        let blob = await res.blob()

        loadImage(
            blob,
            (canvas) => {
                let dataURL = canvas.toDataURL('image/jpeg');
                resolve(removeBase64Prefix(dataURL));
            }, {
                maxWidth: maxWidth,
                canvas: true
            }
        );
    });
}

export default {
    removeBase64Prefix,
    generateThumbnail,
    addBase64Prefix,
};
```

注意，我们在这里移动了`removeBase64Prefix`和`addBase64Prefix`方法。`generateThumbnail`函数获取 imageData -一个 base64 字符串，使用`fetch` API，将其转换为`blob`，然后使用 blueimp 的`loadImage`将其大小更改为`maxWidth`。我们正在从一个新的配置文件中加载`maxWidth`，为了使事情变得整洁，我们已经添加了这个文件- `src/services/config.js`:

```
export default {
    photos: {
        maxWidth: 1000,
        thumbnailMaxWidth: 30
    }
}
```

# 添加图像上传服务

我们将把所有的图片上传流提取到一个服务中，以保持 UI 组件的整洁。添加文件`src/services/image-uploader.js`:

```
import store from "./store";
import cordovaCamera from "./cordova-camera";
import imageOps from "./image-ops";
import config from "./config";

async function uploadImageFromCamera() {
    let base64 = await cordovaCamera.getBase64FromCamera();

    let imageData = imageOps.removeBase64Prefix(base64);

    let thumbnailImageData = await imageOps.generateThumbnail(
        imageData,
        config.photos.thumbnailMaxWidth
    );

    let localPic = {
        url: imageOps.addBase64Prefix(thumbnailImageData),
        uploading: true
    };
    store.addPic(localPic);

    let uploadedPic = await store.state.workerInstance.uploadPicture(
        imageData
    );
    store.updatePicUploaded(localPic, uploadedPic);
}

export default {
    uploadImageFromCamera
}
```

我们在`uploadImageFromCamera`方法中所做的是:从 cordova 相机中获取 base64->使用我们的`imageOps` - >生成缩略图，使用`uploading=true` - >在`state`中生成一个新的`pic`对象，使用 web worker - >将图片上传到 Firebase，当上传完成时更新 pic 的`uploading`状态属性。

# 把它放在一起

现在我们已经添加了所有这些服务，我们需要从我们的组件中调用它们。

在`App.vue`中，我们将使用`mounted`生命周期钩子来初始化 worker，并从保存状态加载保存的图片 URL:

```
<template>
  <div id="q-app">
    <router-view />
  </div>
</template>

<script>
import store from "./services/store.js";

export default {
  name: "App",
  async mounted() {
    store.initWorker();
    store.loadPictures();
  }
};
</script>

<style>
</style>
```

最后，我们将在`Index.vue`中添加查看这一切的界面:

```
<template>
  <q-page class>
    <div class="q-pa-md">
      <div class="row justify-center q-ma-md" v-for="(pic, idx) in pics" :key="idx">
        <div class="col">
          <q-card v-if="pic">
            <q-img spinner-color="white" :src="pic.url">
              <div class="spinner-container" v-if="pic.uploading && !pic.failed">
                <q-spinner color="white" size="4em" />
              </div>
              <div class="spinner-container" v-if="pic.failed">
                <q-icon name="cloud_off" style="font-size: 48px;"></q-icon>
              </div>
            </q-img>
            <q-card-actions align="around">
              <q-btn flat round color="red" icon="favorite" @click="notifyNotImplemented()" />
              <q-btn flat round color="teal" icon="bookmark" @click="notifyNotImplemented()" />
              <q-btn
                flat
                round
                color="primary"
                icon="delete"
                @click="deletePic(idx)"
                :disable="pic.uploading"
              />
            </q-card-actions>
          </q-card>
        </div>
      </div>
    </div>
  </q-page>
</template>

<style scoped>
.spinner-container {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}

</style>

<script>
import store from "../services/store";
import { EventBus } from "../services/event-bus";
import imageUploader from "../services/image-uploader";

export default {
  name: "PageIndex",
  data() {
    return {
      state: store.state
    };
  },
  mounted() {
    EventBus.$off("takePicture");
    EventBus.$on("takePicture", this.uploadImageFromCamera);
  },
  computed: {
    pics() {
      return this.state.pics;
    }
  },
  methods: {
    notifyNotImplemented() {
      this.$q.notify({ message: "Not implemented yet :/" });
    },
    async deletePic(idx) {
      try {
        await store.deletePic(idx);
        this.$q.notify({ message: "Picture deleted." });
      } catch (err) {
        console.error(err);
        this.$q.notify({ message: "Delete failed. Check log." });
      }
    },
    async uploadImageFromCamera() {
      try {
        imageUploader.uploadImageFromCamera();
      } catch (err) {
        console.error("Uploading failed");
        console.dir(err);
        store.updatePicFailed(localPic);
        this.$q.notify({ message: "Uploading failed. Check log." });
      }
    }
  }
};
</script>
```

这是怎么回事:

*   我们添加了一个`q-spinner`来显示图片的`uploading`属性何时为真。
*   我们在点击照片按钮时调用`imageUploader.uploadImageFromCamera`，它处理我们的上传。
*   我们在图片的`q-card`中添加了一些动作(目前只实现了删除)

# 最终应用

就是这样！我们有一个图像上传应用程序，用微调器显示模糊的缩略图，有点像 WhatsApp 的图像上传。使用`quasar dev -m android/ios`运行这一切将显示最终结果:

# 进一步的改进

进一步改进该应用程序的一些想法如下:

*   将缩略图创建工作交给另一个网络工作者
*   用一个虚拟列表组件处理长长的图像列表，比如这个
*   添加一个旋转木马/画廊组件来查看完整尺寸的图像
*   为失败的上传添加重试机制
*   使用[firebase auth](https://firebase.google.com/docs/auth)——这样用户只能删除自己的照片

# 源代码

完整代码在 GitHub 这里:[vue-firebase-image-upload](https://github.com/syonip/vue-firebase-image-upload)。

享受:)

![](img/7534688196b6b786997bb7313ee442b9.png)