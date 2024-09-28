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

**2 Облачные платформы:** Такие как *Google Colab, Kaggle Notebooks, Jupyter Notebooks* на серверах AWS или Azure. Эти платформы предлагают доступ к мощным GPU и большому объему памяти, что делает их отличным выбором для масштабного обучения и экспериментов.

Обработку кода я передам в облако, что и вам советую:

Следующий шаг - Это организация проекта: Структурируйте проект таким образом, чтобы легко находить и управлять файлами. 

Мой пример: оснавная папка с названием проекта BackFreeBot_CV

## BackFreeBot_CV/

├── **BackFreeBot_folderId_Photo/** # исходные данные для обучения и тестирования, обычные фото сделаные на телефон  
    └── имя файлов.jpg  
    
├── **BackFreeBot_folderId_Mask_Canny/** # Маски Canny - Данные для обучения и тестирования, создание маски  
    └── имя файлов.jpg или .png  
    
├── **BackFreeBot_folderId_Mask_Grab/** # Маски Grab - Данные для обучения и тестирования, создание маски  
    └── имя файлов.jpg или .png   
    
├── **BackFreeBot_folderId_Mask_Grab_Mirroredy/** # Зеркальные маски Grab - Данные для обучения и тестирования, создание маски  
    └── имя файлов.jpg или .png  
    
├── **BackFreeBot_folderId_Mask_DeepLabV3/** # Маски DeepLabV3 - Данные для обучения и тестирования, создание маски   
    └── имя файлов.jpg или .png  
    
├── **BackFreeBot_folderId_Mask_Combined/** # Комбинированные маски - объединения данных для обучения и тестирования    
    └── имя файлов.jpg или .png   
    
├── **BackFreeBot_folderId_Resolt/** # должны находиться результаты обработки изображений, полученные после применения модели     
    └── имя файлов.jpg или .png    
    
├── **BackFreeBot_folderId_Finish/** # результаты обработки изображений  
    └── имя файлов.jpg или .png  
    
├── **user_id_timestamp.ipynb**- notebooks/  # Jupyter ноутбук с кодом  

└── **unet_model.pth** - models/ # Сохраненная модель   
 

