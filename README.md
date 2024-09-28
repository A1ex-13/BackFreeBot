# BackFreeBot - бот для удаления фона с изображения

![@BackFree_Bot](https://github.com/A1ex-13/BackFreeBot/blob/main/photo_2024-09-21_10-14-38.jpg)  
🔗 [@BackFree_Bot](https://t.me/BackFree_Bot)

**"Создание собственной модели компьютерного зрения: от идеи до внедрения"**

Эта работа предназначена для всех, кто хочет научиться создавать свою модель компьютерного зрения. 
Подробно разберу весь процесс: от первоначальной идеи до реализации и обучения модели на новых изображениях. 
Вы узнаете, как выбрать и подготовить данные, создать и обучить модель, а также сохранить и использовать её в будущем для распознавания новых объектов. 
Назовем это курсом который поможет вам освоить ключевые навыки в области машинного обучения и компьютерного зрения, необходимые для успешной реализации проектов на практике.

Пропустим процесс определение возможностей вашего  железа — это первый шаг в разработке модели компьютерного зрения. 
Вам стоит задуматься как обрабатывать код и где хранить.



Обработка кода и хранение данных в проекте компьютерного зрения. Есть две среды разработки :

**1 Локальная среда:** Это может быть ваш компьютер с установленными *IDE*, такими как *PyCharm* или *VS Code*. Подходит для небольших экспериментов и разработки. Для работы с большим объемом данных и тренировки моделей может потребоваться мощное оборудование.

**2 Облачные платформы:** Такие как *```Google Colab```, Kaggle Notebooks, Jupyter Notebooks* на серверах AWS или Azure. Эти платформы предлагают доступ к мощным GPU и большому объему памяти, что делает их отличным выбором для масштабного обучения и экспериментов.

Обработку кода я передам в ```облако```, что и вам советую:

Следующий шаг - Это организация проекта: Структурируйте проект таким образом, чтобы легко находить и управлять файлами. 

Мой пример - оснавная папка с названием проекта **BackFreeBot_CV**

## BackFreeBot_CV/

├── **BackFreeBot_folderId_Photo/** # исходные данные для обучения и тестирования, обычные фото сделаные на телефон  
    └── пример файлов от пользователей бота с именем 100001_27.09.2024_22.05.jpg
    
├── **BackFreeBot_folderId_Mask_Canny/** # Маски Canny - Данные для обучения и тестирования, создание маски  
    └── имя файлов mask_canny_new_100001_27.09.2024_22.05.png  
    
├── **BackFreeBot_folderId_Mask_Grab/** # Маски Grab - Данные для обучения и тестирования, создание маски  
    └── имя файлов mask_grab_100001_27.09.2024_22.05.png
    
├── **BackFreeBot_folderId_Mask_Grab_Mirroredy/** # Зеркальные маски Grab - Данные для обучения и тестирования, создание маски  
    └── имя файлов mask_grab_mirrored_100001_27.09.2024_22.05.png  
    
├── **BackFreeBot_folderId_Mask_DeepLabV3/** # Маски DeepLabV3 - Данные для обучения и тестирования, создание маски   
    └── имя файлов 100004_27.09.2024_22.05_deeplabv3.png  
    
├── **BackFreeBot_folderId_Mask_Combined/** # Комбинированные маски - объединения данных для обучения и тестирования    
    └── имя файлов.jpg или .png   
    
├── **BackFreeBot_folderId_Resolt/** # должны находиться результаты обработки изображений, полученные после применения модели     
    └── имя файлов.jpg или .png    
    
├── **BackFreeBot_folderId_Finish/** # результаты обработки изображений  
    └── имя файлов.jpg или .png  
    
├── **user_id_timestamp.ipynb**- notebooks/  # Jupyter ноутбук с кодом  

├── **unet_model.pth** - models/ # Сохраненная модель  

├── **deeplabv3_resnet101.pth** - models/ # Сохраненная модель для DeepLabV3

├── **README.md**  # файл с описанем, и вы это читаете.

├── **model_training_counter.txt**  - счетчик запусков (я сделал каждые 10 раз) для обучения модили deeplabv3_resnet101

└── **photo_2024-09-21_10-14-38.jpg** # 

# ШАГ 1 💻🛠️ 

🔄📂 Упрощаем проверку директорий с помощью словаря и цикла  

🎯💡 Использование словаря : Теперь все директории хранятся в одном месте, что упрощает их проверку и создание, если они отсутствуют.  

🔁🧹 Цикл для проверки : Мы проходим по всем директориям с помощью цикла ```for```, избегая дублирования кода и делая его чище и понятнее.  

📂✅ Запускайте код на Python и вы сразу увидите содержимое существующих папок, а отсутствующие директории будут созданы автоматически!

```python
import os

# Определяем директории
directories = {
    "image_dir": '/BackFreeBot_CV/BackFreeBot_folderId_Photo',
    "mask_grab_dir": '/BackFreeBot_CV/BackFreeBot_folderId_Mask_Grab',
    "mask_grab_mirrored_dir": '/BackFreeBot_CV/BackFreeBot_folderId_Mask_Grab_Mirrored',
    "mask_canny_dir": '//BackFreeBot_CV/BackFreeBot_folderId_Mask_Canny',
    "mask_deeplabv3_dir": '/BackFreeBot_CV/BackFreeBot_folderId_Mask_DeepLabV3',
    "mask_combined_dir": '/BackFreeBot_CV/BackFreeBot_folderId_Mask_Combined',
    "image_resolt_dir": '/BackFreeBot_CV/BackFreeBot_folderId_Resolt',
    "finish_dir": '/BackFreeBot_CV/BackFreeBot_folderId_Finish'
}

# Проверка существования директории
for dir_name, dir_path in directories.items():
    if os.path.exists(dir_path):
        print(f"Директория {dir_name} ({dir_path}) существует. Содержимое директории:")
        print(os.listdir(dir_path))
    else:
        print(f"Директория {dir_name} ({dir_path}) не найдена. Создаем директорию.")
        os.makedirs(dir_path)
```
# ШАГ 2  BackFreeBot_folderId_Mask_Grab & BackFreeBot_folderId_Mask_Grab_Mirrore
🔍✨ Модель GrabCut: Магия сегментации изображений 

GrabCut — это мощный алгоритм сегментации изображений, который позволяет отделять передний план от фона на фото. Изначально разработанный для улучшения пользовательского опыта при редактировании изображений, он использует методы графов и обучения для достижения наилучшего результата.  

🔹 Как это работает?  

📌 Задаем прямоугольник, охватывающий объект.  
🔗 Алгоритм строит граф, где каждый пиксель — это узел.  
✂️ Используя минимальный разрез графа, отделяет передний план от фона.  

🔹 Где применяется?  

📸 Фотография и обработка изображений.  
🎨 Создание масок для художественных эффектов.  
🛠️ Предварительная обработка данных в компьютерном зрении.  

```python  
import os
import cv2
import numpy as np

# Указываем пути к папкам 
image_dir = '/BackFreeBot_CV/BackFreeBot_folderId_Photo'
mask_grab_dir = '/BackFreeBot_CV/BackFreeBot_folderId_Mask_Grab'
mask_grab_mirrored_dir = '/BackFreeBot_CV/BackFreeBot_folderId_Mask_Grab_Mirrored'

# Функция для генерации маски с использованием алгоритма GrabCut
def generate_mask_grabcut(image_path, save_path):
    img = cv2.imread(image_path)
    if img is None:
        print(f"Ошибка: Не удалось загрузить изображение {image_path}. Пропускаем файл.")
        return
    mask = np.zeros(img.shape[:2], np.uint8)
    bgdModel = np.zeros((1, 65), np.float64)
    fgdModel = np.zeros((1, 65), np.float64)

    rect = (10, 10, img.shape[1] - 10, img.shape[0] - 10)  # Определяем прямоугольную область
    cv2.grabCut(img, mask, rect, bgdModel, fgdModel, 5, cv2.GC_INIT_WITH_RECT)

    # Генерируем маску и применяем ее к изображению
    mask2 = np.where((mask == 2) | (mask == 0), 0, 1).astype('uint8')
    masked_img = img * mask2[:, :, np.newaxis]
    cv2.imwrite(save_path, masked_img)

# Функция для генерации маски на зеркальном изображении
def generate_mask_grabcut_mirrored(image_path, save_path_mirrored):
    img = cv2.imread(image_path)
    if img is None:
        print(f"Ошибка: Не удалось загрузить изображение {image_path}. Пропускаем файл.")
        return

    mirrored_img = cv2.flip(img, 1)  # Зеркалируем изображение по горизонтали
    mask = np.zeros(mirrored_img.shape[:2], np.uint8)
    bgdModel = np.zeros((1, 65), np.float64)
    fgdModel = np.zeros((1, 65), np.float64)

    rect = (10, 10, mirrored_img.shape[1] - 10, mirrored_img.shape[0] - 10)  # Определяем прямоугольную область
    cv2.grabCut(mirrored_img, mask, rect, bgdModel, fgdModel, 5, cv2.GC_INIT_WITH_RECT)

    # Генерируем маску и применяем ее к зеркальному изображению
    mask2 = np.where((mask == 2) | (mask == 0), 0, 1).astype('uint8')
    masked_mirrored_img = mirrored_img * mask2[:, :, np.newaxis]
    cv2.imwrite(save_path_mirrored, masked_mirrored_img)

# Создаем директорию для зеркальных масок, если она не существует
if not os.path.exists(mask_grab_mirrored_dir):
    os.makedirs(mask_grab_mirrored_dir)

# Генерация масок для всех изображений в папке
for image_file in os.listdir(image_dir):
    image_path = os.path.join(image_dir, image_file)

    # Генерация маски для оригинального изображения
    mask_path = os.path.join(mask_grab_dir, f'mask_grab_{os.path.splitext(image_file)[0]}.png')
    try:
        generate_mask_grabcut(image_path, mask_path)
    except Exception as e:
        print(f"Ошибка при обработке {image_file}: {e}")

    # Генерация маски для зеркального изображения
    mask_mirrored_path = os.path.join(mask_grab_mirrored_dir, f'mask_grab_mirrored_{os.path.splitext(image_file)[0]}.png')
    try:
        generate_mask_grabcut_mirrored(image_path, mask_mirrored_path)
    except Exception as e:
        print(f"Ошибка при обработке {image_file}: {e}")

print("Маски Grab и зеркальные маски успешно сгенерированы.")
```
# ШАГ 3 BackFreeBot_folderId_Mask_Canny

🎯🔍 Модель Canny: Идеальные границы на изображениях   

Алгоритм Canny — это классический и надежный инструмент для обнаружения границ на изображениях. Он использует пошаговый подход, чтобы выделить контуры объектов, минимизируя шум и улучшая четкость границ.  

🔹 Как это работает?  

🧽 Шумоподавление: Размывает изображение, чтобы уменьшить мелкие детали.  
🚦 Вычисление градиентов: Определяет интенсивность и направление границ.  
✂️ Подавление немаксимумов: Убирает ложные пиксели, чтобы оставить только четкие границы.  
✅ Двойной порог: Разделяет слабые и сильные границы для лучшего результата.  

🔹 Где применяется?  

🏞️ Обработка фотографий и создание контурных карт.    
🔍 Распознавание объектов и анализ изображений.  
🤖 Приложения в робототехнике и компьютерном зрении.  

```python
import os
import cv2
import numpy as np

# Указываем путь к папке на Google Диске
image_dir = '/BackFreeBot_CV/BackFreeBot_folderId_Photo'
mask_canny_dir = '/BackFreeBot_CV/BackFreeBot_folderId_Mask_Canny'

# Функция для удаления фона с использованием метода Кэнни и контуров
def remove_background_canny(image_path, save_path):
    img = cv2.imread(image_path)
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

    # Применение оператора Кэнни для обнаружения границ
    edges = cv2.Canny(gray, 100, 200)

    # Поиск контуров
    contours, _ = cv2.findContours(edges, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

    # Создание маски (залитая область внутри контуров)
    mask = np.zeros_like(img)

    # Заливаем область внутри контуров
    cv2.drawContours(mask, contours, -1, (255, 255, 255), thickness=cv2.FILLED)

    # Опционально: улучшение маски с помощью морфологии (расширение контуров)
    kernel = np.ones((5, 5), np.uint8)
    mask = cv2.morphologyEx(mask, cv2.MORPH_CLOSE, kernel)

    # Применение маски к изображению (удаление фона)
    result = cv2.bitwise_and(img, mask)

    # Сохранение результата
    cv2.imwrite(save_path, result)

# Генерация масок для всех изображений в папке
for image_file in os.listdir(image_dir):
    # Проверка, что файл имеет расширение .jpg
    if image_file.endswith('.jpg'):
        image_path = os.path.join(image_dir, image_file)
        mask_path = os.path.join(mask_canny_dir, 'mask_canny_new_' + os.path.splitext(image_file)[0] + '.png')  # Сохраняем в .png

        # Применение функции для каждого изображения
        remove_background_canny(image_path, mask_path)

print("Маски canny успешно сгенерированы.")
```
# ШАГ 4 🤖🖥️ BackFreeBot_folderId_Mask_DeepLabV3  
## ШАГ 4.1  Сгенерировать маски DeepLabV3  
🖼️🔍 DeepLabV3: Продвинутый мастер сегментации изображений   

DeepLabV3 — это мощная модель глубокого обучения, созданная для сегментации изображений на уровне пикселей. Она позволяет выделять объекты на изображении с высокой точностью, разделяя его на отдельные классы.  [Скачать deeplabv3_resnet101.pth](https://download.pytorch.org/models/deeplabv3_resnet101_coco-586e8e8f.pth)  

🔹 Как это работает?  

🧠 Глубокая сверточная сеть: Использует расширенные сверточные операции для захвата контекста изображения.  
🔗 Атропное свертки: Обрабатывает объекты разного масштаба, обеспечивая высокую детализацию.  
🗺️ Парсинг сцены: Разбивает изображение на семантические сегменты, распознавая даже мелкие детали.  

🔹 Где применяется?  

🛣️ Разметка дорог для автономного вождения.  
🏡 Сегментация зданий и объектов на спутниковых снимках.  
🌳 Выделение природных объектов на изображениях.  
🚀DeepLabV3 — ваш выбор для задач, где важна каждая деталь! 

🤖🖥️ на этом шаге используем структуру

    ├── BackFreeBot_folderId_Mask_DeepLabV3/ # Маски DeepLabV3 - папка с файлами для обучения и создание маски   
        └── имя файлов 100004_27.09.2024_22.05_deeplabv3.png     
    ├── deeplabv3_resnet101.pth - предобученная модель DeepLabV3 с использованием ResNet-101.  
    ├── model_training_counter.txt  - счетчик запусков (автоматизация сделал каждые 10 раз) для обучения модили deeplabv3_resnet101    
    └── pytorch/vision:v0.10.0 — это версия Docker-образа, содержащая PyTorch и библиотеку torchvision версии 0.10.0. Этот образ включает в себя все необходимые зависимости для работы с нейронными сетями и задачами компьютерного зрения, такими как загрузка и предобработка изображений, а также использование предобученных моделей для классификации, детекции объектов и сегментации.    

🔹 Что входит в этот образ который нужно [Скачать Docker-образ pytorch/vision:v0.10.0](https://hub.docker.com/r/pytorch/vision/tags?page=1&name=v0.10.0)  

🔧 PyTorch: Версия 1.9.0, обеспечивает поддержку основных операций машинного обучения и нейронных сетей.  
📷 torchvision: Версия 0.10.0, включает наборы данных, предобученные модели и утилиты для работы с изображениями.  
🛠️ CUDA (если поддерживается): Образ оптимизирован для работы с GPU, что позволяет значительно ускорить обучение моделей.  

🔹 Где применяется?  

👨‍💻 Разработка и обучение моделей глубокого обучения на основе PyTorch.  
🖼️ Решение задач компьютерного зрения, таких как классификация и сегментация изображений.  
🤖 Прототипирование и тестирование нейронных сетей в контейнерах.  
🌟 Этот Docker-образ удобен для быстрого развертывания окружения и воспроизведения результатов на различных машинах.  

```python
import torch
import torchvision.transforms as T
from PIL import Image
import numpy as np
import os

# Путь для сохранения модели
model_save_path = '/BackFreeBot_CV/deeplabv3_resnet101.pth'
# Путь для сохранения счетчика запусков
counter_path = '/BackFreeBot_CV/model_training_counter.txt'

# Проверяем, существует ли файл счетчика и читаем значение, иначе устанавливаем счетчик в 0
if os.path.exists(counter_path):
    with open(counter_path, 'r') as f:
        try:
            training_counter = int(f.read().strip())
        except ValueError:
            training_counter = 0
else:
    training_counter = 0

# Триггер для обучения модели, если True то будет каждый раз обучаться
should_train_model = False

# Увеличиваем счетчик и сохраняем его в файл
training_counter += 1
with open(counter_path, 'w') as f:
    f.write(str(training_counter))

# Проверяем, нужно ли обучать модель (каждый 10-й запуск)
if training_counter >= 10:
    should_train_model = True
    training_counter = 0  # Сбрасываем счетчик после обучения
    with open(counter_path, 'w') as f:
        f.write(str(training_counter))  # Обновляем счетчик в файле

# Функция для загрузки модели из сохраненного состояния с параметром strict=False
def load_model(model_path, device):
    model = torch.hub.load('pytorch/vision:v0.10.0', 'deeplabv3_resnet101', pretrained=False)  # Загружаем модель без предобученных весов
    state_dict = torch.load(model_path, map_location=device)
    model.load_state_dict(state_dict, strict=False)  # strict=False позволяет игнорировать несовпадающие ключи
    model.to(device)
    model.eval()
    return model

# Загрузка или обучение модели в зависимости от триггера
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')

if should_train_model:
    # Загрузка предобученной модели DeepLabV3 и отправка на GPU (если доступен)
    model = torch.hub.load('pytorch/vision:v0.10.0', 'deeplabv3_resnet101', pretrained=True)
    model.to(device)
    
    # Сохранение модели в указанную директорию на Google Диске
    torch.save(model.state_dict(), model_save_path)
    print(f"Модель успешно обучена и сохранена в {model_save_path}")
else:
    # Загрузка модели из сохраненного состояния
    model = load_model(model_save_path, device)
    print("Модель загружена из сохраненного состояния.")

# Преобразование изображения в тензор для модели и отправка на GPU
def preprocess(image_path):
    input_image = Image.open(image_path)
    preprocess = T.Compose([
        T.ToTensor(),
        T.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]),
    ])
    input_tensor = preprocess(input_image).to(device)  # Отправляем на GPU
    input_batch = input_tensor.unsqueeze(0)  # добавляем размер batch
    return input_batch, input_image

# Функция для предсказания маски
def get_segmentation_mask(image_path):
    input_batch, input_image = preprocess(image_path)

    with torch.no_grad():
        output = model(input_batch)['out'][0]
    output_predictions = output.argmax(0).byte().cpu().numpy()  # Возвращаем на CPU для обработки

    # Создаём маски для разных классов
    masks = {
        'person': output_predictions == 15,  # человек
        'clothing': output_predictions == 17,  # одежда
        'shoes': output_predictions == 18,  # обувь
        'bag': output_predictions == 24,  # сумка
        'hat': output_predictions == 25,  # шляпа
        # Добавьте сюда дополнительные классы, если номера классов известны
        # 'socks': output_predictions == <номер класса>,  # носки
        # 'underwear': output_predictions == <номер класса>,  # нижнее белье
        # 'pants': output_predictions == <номер класса>,  # штаны
        # 'shorts': output_predictions == <номер класса>,  # шорты
        # 'scarf': output_predictions == <номер класса>  # шарф
    }

    return masks, input_image

# Основной цикл обработки изображений
image_dir = '/BackFreeBot_CV/BackFreeBot_folderId_Photo'
mask_deeplabv3_dir = '/BackFreeBot_CV/BackFreeBot_folderId_Mask_DeepLabV3'

# Создаем директорию для масок DeepLabV3, если она не существует
if not os.path.exists(mask_deeplabv3_dir):
    os.makedirs(mask_deeplabv3_dir)

# Генерация масок для всех изображений в папке
for image_file in os.listdir(image_dir):
    # Проверка корректности имени файла (например, "100001_27.09.2024_22.05.jpg")
    if image_file.endswith('.jpg') and len(image_file.split('_')) == 3:
        image_path = os.path.join(image_dir, image_file)

        # Применение функции для каждого изображения
        masks, input_image = get_segmentation_mask(image_path)

        # Обработка изображения и масок
        input_image = np.array(input_image)

        # Определяем доминирующий класс на основе площади маски
        dominant_class = None
        max_area = 0

        for mask_name, mask in masks.items():
            area = mask.sum()  # Площадь маски (количество пикселей)
            if area > max_area:
                max_area = area
                dominant_class = mask_name

        # Если доминирующий класс определен, создаем комбинированную маску для этого класса
        if dominant_class:
            dominant_mask = masks[dominant_class].astype(np.uint8) * 255
            combined_image = Image.fromarray(dominant_mask)

            # Генерация имени нового файла для комбинированной маски
            combined_filename = f'{os.path.splitext(image_file)[0]}_combined_deeplabv3.png'

            # Сохраняем комбинированную маску в указанную директорию
            save_path = os.path.join(mask_deeplabv3_dir, combined_filename)
            combined_image.save(save_path)
            print(f"Сохранена комбинированная маска {dominant_class} для {image_file}: {save_path}")

print("Маски DeepLabV3 успешно сгенерированы.")
```
## ШАГ 4.2 обучение модели deeplabv3_resnet101

```python
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import DataLoader, Dataset
import torchvision.transforms as T
from PIL import Image
import os

# Триггер для запуска обучения модели
should_train_model = True  # Измените на False, чтобы пропустить обучение

# Путь для сохранения и загрузки модели
model_save_path = '/BackFreeBot_CV/deeplabv3_resnet101.pth'

# Функция для загрузки модели из сохраненного состояния
def load_model(model_path, device):
    model = torch.hub.load('pytorch/vision:v0.10.0', 'deeplabv3_resnet101', pretrained=False)
    state_dict = torch.load(model_path, map_location=device)
    model.load_state_dict(state_dict, strict=False)  # strict=False, чтобы игнорировать несовпадающие слои
    model.to(device)
    return model

# Кастомный датасет для ваших данных
class CustomSegmentationDataset(Dataset):
    def __init__(self, image_dir, mask_dir, transform=None):
        self.image_dir = image_dir
        self.mask_dir = mask_dir
        self.transform = transform
        self.images = [file for file in os.listdir(image_dir) if file.endswith('.jpg')]

    def __len__(self):
        return len(self.images)

    def __getitem__(self, idx):
        image_path = os.path.join(self.image_dir, self.images[idx])
        mask_path = os.path.join(self.mask_dir, os.path.splitext(self.images[idx])[0] + '_mask.png')

        image = Image.open(image_path).convert("RGB")
        mask = Image.open(mask_path).convert("L")  # Маска в градациях серого

        if self.transform:
            image = self.transform(image)
            mask = T.ToTensor()(mask).long()  # Преобразуем маску в тензор

        return image, mask

# Преобразования для изображений и масок
transform = T.Compose([
    T.ToTensor(),
    T.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225])
])

# Путь к изображениям и маскам
image_dir = '/BackFreeBot_CV/BackFreeBot_folderId_Photo'
mask_dir = '/BackFreeBot_CV/BackFreeBot_folderId_Mask_DeepLabV3'

if should_train_model:
    # Создание датасета и загрузчика данных
    dataset = CustomSegmentationDataset(image_dir=image_dir, mask_dir=mask_dir, transform=transform)
    data_loader = DataLoader(dataset, batch_size=4, shuffle=True, num_workers=2)

    # Загрузка модели на устройство (GPU или CPU)
    device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
    model = load_model(model_save_path, device)

    # Определение функции потерь и оптимизатора
    criterion = nn.CrossEntropyLoss()  # Функция потерь для многоклассовой сегментации
    optimizer = optim.Adam(model.parameters(), lr=1e-4)

    # Количество эпох для дообучения
    num_epochs = 10

    # Цикл дообучения
    for epoch in range(num_epochs):
        model.train()
        running_loss = 0.0

        for images, masks in data_loader:
            images, masks = images.to(device), masks.to(device)

            optimizer.zero_grad()

            outputs = model(images)['out']
            loss = criterion(outputs, masks.squeeze(1))  # Сжимаем размерность маски для функции потерь

            loss.backward()
            optimizer.step()

            running_loss += loss.item()

        print(f"Эпоха [{epoch + 1}/{num_epochs}], Потери: {running_loss / len(data_loader)}")

    # Сохранение дообученной модели
    torch.save(model.state_dict(), model_save_path)
    print(f"Дообученная модель успешно сохранена в {model_save_path}")
else:
    print("Обучение модели пропущено.")
```
