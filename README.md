# Сортировка мусора
Проект реализован в рамках соревнования [Kaggle "Сортировка Мусора"](https://www.kaggle.com/competitions/waste-detection)
## Описание проекта 

**Общая информация**

Заказчик компания Renue – IT-компания из Екатеринбурга, разрабатывает на заказ высоконагруженные и отказоустойчивые решения для крупных российских заказчиков, для бизнеса и государства. В мире, где переработка отходов играет важную роль в защите окружающей среды, сортировка мусора является ключевым элементом в повышении эффективности перерабатывающих заводов. 

**Цель проекта**

Создать модель, которая сможет распознавать различные виды отходов на основании изображений с конвейера.

**Целевая метрика** - mean average precision (mAP).

## Модели
Для выполнения поставленной задачи после анализа существующих подходов была выбрана модель YOLO 11 от компании Ultralytics. Были рассмотрены следующие варианты этой модели.

|Итерация|Версия|Количество эпох|Заморозка слоев|Доп агументации|mAP@50:95 val|mAP test|
|-|----|------|--------|----|----|----|
|1|yolo11m.pt|50|Только последний с заменой числа классов на 15|нет|0.876|0.85512|
|2|yolo11m.pt|100|Только последний с заменой числа классов на 15|mixup=0.5,flipud=0.5|0.87|0.84857|
|3|yolo11m.pt|50|Заморозка первых 10 слоев (backbone)|нет|0.861|0.83727|
|4|yolo11s.pt|50|Только последний с заменой числа классов на 15|нет|0.876|0.85503|
|5|yolo11l.pt|100|Только последний с заменой числа классов на 15|нет|0.873|0.84910|

Наилучшей оказалась модель версии yolo11m.pt, обученная на 50-ти эпохах, без применения заморзки слоев и дополнительных аугментаций. Данная модель показала значение mAP=0.876 на валидации и mAP=0.85512 на тесте (Private Score в Submissions).

## Получение предсказаний с помощью полученной модели
- скопировать данный репозиторий;
- скачать [веса модели](https://drive.google.com/drive/folders/1zjz8tPwV-9NDO_zPXTkxW2ucISUnf1UR?usp=sharing);
- в файле [02_garbage-detection-test.ipynb] задать пути к папке с изображениями, к папке для предсказаний и папке со скачанными весами моделей в переменных `test_img_path`, `output_csv_path` и `model` соответственно. Запустить этот файл для получения предсказаний.
