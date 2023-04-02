# 如何使用 HTML 画布通过 React 编辑图片上传

> 原文：<https://javascript.plainenglish.io/using-html-canvas-to-edit-image-uploads-with-react-ba1d377ea3ff?source=collection_archive---------0----------------------->

有几个使用 canvas 来优化、更改、调整大小或仅仅显示用户上传的用例。既然 React 是我们选择的库，那么让我们深入研究如何创建一个简单而强大的钩子来实现预期的应用程序行为。

![](img/dc23fe7ab6d2588889cb9ebc0dd8aa2f.png)

[https://i1.wp.com/onaircode.com/wp-content/uploads/2018/03/HTML5-Canvas.jpg?resize=1280%2C640&ssl=1](https://i1.wp.com/onaircode.com/wp-content/uploads/2018/03/HTML5-Canvas.jpg?resize=1280%2C640&ssl=1)

让我们想象一下，我们正在用 React 构建某种 web 应用程序，碰巧我们的应用程序有一个用户资料部分，用户可以在那里添加封面/资料照片，以便个性化他们的资料。

当然，用户有能力导入他们的个人资料，在这种情况下，我们需要能够复制他们的照片并上传到我们的系统。

让我们将我们的钩子命名为 **useCanvasImage.js**

首先，我们需要考虑从某个 URL 创建一个实际的图像元素，可以是本地的，也可以是外部的。

```
*const* createImage = (*url*) =>
  *new* Promise((*resolve*, *reject*) => {
    *const* image = *new* Image();
    image.addEventListener('load', () => *resolve*(image));
    image.addEventListener('error', *error* => *reject*(*error*));
    image.setAttribute('crossOrigin', 'anonymous');
    image.src = *url*;
  });
```

好了，现在我们有了这个漂亮的工具，我们可以开始构建钩子了。

正如我们在开始时谈到的，我们希望能够降低图像大小和质量。因此，让我们创建几个常数作为默认校正因子。

```
*const* SIZE_REDUCTION_FACTOR = 0.125;
*const* QUALITY_REDUCTION_FACTOR = 0.4;
```

我们还将使用户能够裁剪图像，所以我们将引入带有关于裁剪区域的位置和大小的数据的 **pixelCrop** 参数。

现在我们准备写钩子了。

```
*const* createImage = (*url*): *any* =>
  *new* Promise((*resolve*, *reject*) => {
    *const* image = *new* Image();
    image.addEventListener('load', () => *resolve*(image));
    image.addEventListener('error', *error* => *reject*(*error*));
    image.setAttribute('crossOrigin', 'anonymous');
    image.src = *url*;
  });

*const* getRadianAngle = *degreeValue* => {
  *return* (*degreeValue* * Math.PI) / 180;
};

*const* exportFromCanvas = *async* (
  *canvas*,
  *qualityReductionFactor*,
  *exportAsBlob* ) => {
  *return exportAsBlob* ? *new* Promise(*resolve* => {
        *canvas*.toBlob(
          *file* => {
            *resolve*(URL.createObjectURL(*file*));
          },
          'image/jpeg',
          *qualityReductionFactor* );
      })
    : *canvas*.toDataURL('image/jpeg');
};

*const* SIZE_REDUCTION_FACTOR = 0.125;
*const* QUALITY_REDUCTION_FACTOR = 0.4;

*export const* useCanvasImage = (
  *reductionFactor* = SIZE_REDUCTION_FACTOR,
  *qualityReductionFactor* = QUALITY_REDUCTION_FACTOR,
  *exportAsBlob* = *true* ) => {
  *const* getImage = *async* (
    *imageSrc*, 
    *pixelCrop* = null, 
    *rotation* = 0
) => {
    *const* image = *await* createImage(*imageSrc*);
    *const* canvas = document.createElement('canvas');
    *const* ctx = canvas.getContext('2d') *as any*;

    *if* (!*pixelCrop*) {
      *pixelCrop* = {
        width: image.width,
        height: image.height,
        x: 0,
        y: 0,
      };
    }
    *const* safeArea = Math.max(image.width, image.height);

    canvas.width = safeArea;
    canvas.height = safeArea;

    ctx.translate(
      safeArea * *reductionFactor*,
      safeArea * *reductionFactor* );
    ctx.rotate(getRadianAngle(*rotation*));
    ctx.translate(
      -safeArea * *reductionFactor*,
      -safeArea * *reductionFactor* );
    ctx.drawImage(
      image,
      safeArea * *reductionFactor* - image.width * *reductionFactor*,
      safeArea * *reductionFactor* - image.height * *reductionFactor* );

    *const* data = *ctx*.getImageData(0, 0, *safeArea*, *safeArea*);

    canvas.width = *pixelCrop*.width;
    canvas.height = *pixelCrop*.height;

    ctx.putImageData(
      data,
      0 -
        safeArea * *reductionFactor* +
        image.width * *reductionFactor* -
        *pixelCrop*.x,
      0 -
        safeArea * *reductionFactor* +
        image.height * *reductionFactor* -
        *pixelCrop*.y
    );

    *return* exportFromCanvas(
      canvas,
      *qualityReductionFactor*,
      *exportAsBlob* );
  };

  *return* { getImage };
};
```

就是这样。我们现在可以在 React 应用程序中使用钩子了。

让我们首先来看看这样一种情况，我们从其他地方导入一个图像，我们想把它转换成画布，这样我们就可以存储我们自己的副本。要做到这一点，我们需要做的就是为我们的钩子提供外部 URL。

```
const { getImage } = useCanvasImage()*const* getUserExternalAccount = *async* (*url*) => {
  *const* { data } = *await* axios.get(url); 
  ...
  const imageFromCanvas = *await* getImage(data.avatar_url)
  ...
}; data.avatar_url = *await* getImage(data.avatar_url)

};
```

是的，就是这么简单！

既然我们已经解决了这个问题，现在让我们来处理来自本地磁盘的用户上传。为了减少代码，我将使用 Ant Design 的上传组件。

```
*import* React*from* 'react';
*import* { Upload } *from* 'antd';

*import* { useCanvasImage } *from* '...';

*export const* FormUploadField = ({ *onChange* }) => {
  *const* { getImage } = useCanvasImage(); const handleBeforeUpload = *file* => {
    *const* reader = *new* FileReader();
    reader.onload = *async e* => {
      *if* (*e*.target) {
        *onChange*(*await* getImage(*e*.target.result));
      }
    };
    reader.readAsDataURL(*file*);

    *// Prevent upload
    return false*;
  } *return* (
      <Upload
        *accept*={'.png,.jpeg,.jpg'}
        *beforeUpload*={handleBeforeUpload}
        *onChange*={() => {}}
      >
      </Upload>
  );
};
```

是的，就是这么简单！

自从我们添加了裁剪和旋转功能，现在我们的库实际上可以即插即用地使用令人敬畏的 React 裁剪库 **react-easy-crop。**让我们添加一个图像模型，这样用户可以使用 react-easy-crop 与画布图像进行交互，然后使用我们强大的钩子创建一个实际的图像。

```
*import* React, { useCallback, useState} *from* 'react';
*import* Cropper *from* 'react-easy-crop';
*import* { Button, Icon } *from* 'antd';*import* { useCanvasImage } *from* '...';

*export const* ImageModal = ({
  *visible*,
  *img*,
  *onCrop*,
  *onClose*,
  *aspectRatio* }) => {
  *const* [crop, setCrop] = useState({ x: 0, y: 0 });
  *const* [rotation, setRotation] = useState(0);
  *const* [zoom, setZoom] = useState(1);
  *const* [croppedAreaPixels, setCroppedAreaPixels] = useState(*null*);

  *const* { getImage} = useCanvasImage();
 *const* onCropComplete = useCallback((*_*, *croppedAreaPixels*) => {
    setCroppedAreaPixels(*croppedAreaPixels*);
  }, []);

  *const* showCroppedImage = useCallback(*async* () => {
    *const* croppedImage = *await* getImage(
      *img*,
      croppedAreaPixels,
      rotation
    );
    *onCrop*(croppedImage);
    *onClose*();
  }, [croppedAreaPixels, rotation]); *if* (!*visible* || !*img*) {
    *return* <></>;
  }

  *return* (
    <div *className*={'modal-wrapper translucent'}>
      <div *className*={'image-modal'}>
        <span *className*={'modal__close'} *onClick*={*onClose*}>
          Close <Icon *type*="close" />
        </span>
        <div *className*={'image-modal__cropper'}>
          <Cropper
            *image*={*img*}
            *crop*={crop}
            *rotation*={rotation}
            *zoom*={zoom}
            *aspect*={*aspectRatio*}
            *onCropChange*={setCrop}
            *onRotationChange*={setRotation}
            *onCropComplete*={onCropComplete}
            *onZoomChange*={setZoom}
          />
        </div>
        <div *className*={'image-modal__actions'}>
          <Button *onClick*={showCroppedImage}>
            Done
          </Button>
        </div>
      </div>
    </div>
  );
};
```

是的，就是这么简单！

*希望这篇帖子有所帮助，* ***编码快乐！***

*更多内容请看*[*plain English . io*](http://plainenglish.io/)