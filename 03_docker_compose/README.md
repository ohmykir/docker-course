# ЛР: Redis + Rocket Counter. От `docker run` к `docker compose` и масштабированию

## Часть 1. Запуск вручную

1. Запускаем контейнер redis из образа redis и проверяем, что он в состоянии Up:
<img width="1090" height="435" alt="image" src="https://github.com/user-attachments/assets/b7d5cc69-1e38-4c6a-ad90-5ac646899f98" />

2. Запускаем контейнер rocketcounter из образа rotorocloud/rocket-counter и связываем при помощи --link: 
<img width="1090" height="267" alt="image" src="https://github.com/user-attachments/assets/e21f4b67-bf30-421c-afb3-d41ec248ef94" />

   **Проверка:**
   <img width="1090" height="582" alt="image" src="https://github.com/user-attachments/assets/40148402-ca49-4491-a875-785b1abe9d9a" />
    в docker терминале другой curl поэтому переходим в cmd (head в Windows нет, поэтому выводим весь текст)
   
  Убеждаемся, что число растет: 
  
  <img width="500" height="360" alt="image" src="https://github.com/user-attachments/assets/ca8be1fd-d5cf-4e18-9117-ced114ae0b07" /> 
  <img width="500" height="350" alt="image" src="https://github.com/user-attachments/assets/a6714d87-f28b-4a7f-8494-c75fff40d6cf" />

3. Удаляем контейнеры:
   <img width="670" height="228" alt="image" src="https://github.com/user-attachments/assets/6db11537-f735-4d44-af6d-bf9288dc43ad" />

## Часть 2. Описание стека в 'docker-compose.yaml'

1 и 2. Создаем каталог и файл 'docker-compose.yaml':
   <img width="872" height="341" alt="image" src="https://github.com/user-attachments/assets/6d9454e9-4a36-4cba-b984-3ce0321d3167" />

3. Запускаем стек командой "docker compose up -d" и проверяем:
   <img width="1090" height="134" alt="image" src="https://github.com/user-attachments/assets/d47bfa43-d083-43bd-82f8-30752f7ced0a" />
   <img width="1090" height="552" alt="image" src="https://github.com/user-attachments/assets/1c433011-342a-4898-bfc3-07184a2bcdbf" />
   <img width="712" height="494" alt="image" src="https://github.com/user-attachments/assets/69718c1a-336c-442d-8caa-5960e098b66f" />


## Часть 3. Масштабирование приложения

Меняем наш docker-compose.yaml файл 
<img width="813" height="328" alt="image" src="https://github.com/user-attachments/assets/701f5b0f-2645-4a24-8655-9dcd4d9cea0f" />

И масштабируем: 
<img width="1090" height="171" alt="image" src="https://github.com/user-attachments/assets/84fca27d-a974-4992-9dc5-630c29f66fd0" />

Проверяем: 
<img width="1090" height="423" alt="image" src="https://github.com/user-attachments/assets/2d7a9c17-6dbc-4fe9-9b58-718d4e4cfa52" />
Счетчик общий

## Часть 4. Полное уничтожение стека

Командой "docker compose down -v" уничтожаем стек и проверяем, что все пусто: 
<img width="1090" height="176" alt="image" src="https://github.com/user-attachments/assets/0a5c5022-1048-485c-95e4-ee4d2b5d78da" />

## Вопросы

1. Зачем в Ч.1 использовался `--link` и почему в Compose он не нужен?

   Ответ: `--link ` нужен для создания связи между контейнерами, а в compose он не нужен так как сервисы в одном docker-compose.yaml находятся уже в одной сети, т.к. Compose создает ее автоматически.
2. Чем отличается `image` от `build` в Compose?

   Ответ: `image` указывает готовый образ из которого собирается контейнер, а `build` указывает на директорию с Dockerfile, который собирает образ.  
3. Почему нельзя масштабировать сервис с публикацией фиксированного порта на одном хосте? Перечислите способы обойти это ограничение.
   
   Ответ: Потому что нельзя запустить несколько контейнеров на одном фиксированном порте. Однако можно указать динамические порты или запускать на разных хостах.
4. Что делает `docker compose down -v` и чем отличается от `down` без `-v`?
   
   Ответ: `docker compose down` останавливает и удаляет контейнеры, удаляет сети. Флаг `-v` удаляет тома определенные в docker-compose.yaml.
5. Как в Compose обеспечивается сетевое имя сервиса для обращения из другого сервиса?
   
   Ответ: Compose при запуске автоматически создает сеть для проекта, каждый сервис получает DNS-имя, совпадающее с его именем в конфигурации и Docker запускает DNS-сервер внутри сети проекта.
   
