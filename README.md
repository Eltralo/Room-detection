# FloorPlan Detection (Walls & Rooms)


Этот репозиторий содержит код для детекции стен и комнат на архитектурных поэтажных планах с использованием [MMDetection](https://github.com/open-mmlab/mmdetection).

Результаты работы можно увидеть в презентации []

Внутри:
- конфигурация модели на базе **Faster R-CNN + ResNet-50 + FPN**;
- анализ слабых мест;
- рекомендации по расширению модели (двери/окна) и по обработке текстовых размеров (OCR).

> Внимание: датасет и веса модели **не входят** в репозиторий.  
> Вы можете использовать свои данные в COCO-формате и собственные чекпоинты.

## для данной модели представлены классы wall и room


## Архитектура модели

#```mermaid
graph TD
    A[FasterRCNN] --> B[Backbone: ResNet]
    A --> C[Neck: FPN]
    A --> D[RPN Head]
    A --> E[RoI Head]

    B --> B1[Stem: Conv7x7 + BN + ReLU + MaxPool]
    B --> B2[Layer1: 3 Bottlenecks]
    B --> B3[Layer2: 4 Bottlenecks]
    B --> B4[Layer3: 6 Bottlenecks]
    B --> B5[Layer4: 3 Bottlenecks]

    C --> C1[Lateral Convs: 256 from 2048/1024/512/256]
    C --> C2[FPN Convs: 5 layers of Conv3x3 256]

    D --> D1[RPN Conv: 256 to 256]
    D --> D2[RPN Cls: 256 to 3 anchors]
    D --> D3[RPN Reg: 256 to 12 coords (3x4)]

    E --> E1[BBox RoI Extractor: RoIAlign 7x7 at 4 scales]
    E --> E2[BBox Head: Shared2FC]
    E2 --> E2a[FC1: 12544 to 1024]
    E2 --> E2b[FC2: 1024 to 1024]
    E2 --> E2c[FC Cls: 1024 to 2 classes]
    E2 --> E2d[FC Reg: 1024 to 8 coords (2x4)]
```



