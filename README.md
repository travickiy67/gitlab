# Травицкий Сергей
# Домашнее задание к занятию «GitLab»

### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` [репозитория c шаблоном решения](https://github.com/netology-code/sys-pattern-homework) к себе в GitHub и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw.
   2. Выполните клонирование этого репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите сверху название занятия, ваши фамилию и имя;
      - в каждом задании добавьте решение в требуемом виде — текст, код, скриншоты, ссылка.
      - для корректного добавления скриншотов используйте [инструкцию «Как вставить скриншот в шаблон с решением»](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md);
      - при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md).
   4. После завершения работы над домашним заданием сделайте коммит `git commit -m "comment"` и отправьте его на GitHub `git push origin`.
   5. Для проверки домашнего задания в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем GitHub.
   6. Любые вопросы по выполнению заданий задавайте в чате учебной группы или в разделе «Вопросы по заданию» в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!

---

### Задание 1

**Что нужно сделать:**

1. Разверните GitLab локально, используя Vagrantfile и инструкцию, описанные в [этом репозитории](https://github.com/netology-code/sdvps-materials/tree/main/gitlab).   
2. Создайте новый проект и пустой репозиторий в нём.
3. Зарегистрируйте gitlab-runner для этого проекта и запустите его в режиме Docker. Раннер можно регистрировать и запускать на той же виртуальной машине, на которой запущен GitLab.

В качестве ответа в репозиторий шаблона с решением добавьте скриншоты с настройками раннера в проекте.

**Развернул gitlab**  

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img1.1.png)  

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img1.2.png)  

**Создал проект**

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img1.3.png)  

**Регистрация runner**

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img1.4.png)  

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img1.5.png)  

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img1.7.png)  

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img1.8.png)  
 
---

### Задание 2

**Что нужно сделать:** 

1. Запушьте [репозиторий](https://github.com/netology-code/sdvps-materials/tree/main/gitlab) на GitLab, изменив origin. Это изучалось на занятии по Git.
2. Создайте .gitlab-ci.yml, описав в нём все необходимые, на ваш взгляд, этапы.

В качестве ответа в шаблон с решением добавьте: 
   
 * файл gitlab-ci.yml для своего проекта или вставьте код в соответствующее поле в шаблоне; 
 * скриншоты с успешно собранными сборками.

**Во втором заданиии пришлось поднимать две машины в yndex-cloud**  
Ввиду того что на машинах при перезапуске меняется ip, доменное имя прописыват не стал.  

на второй машине поднял sonarqube.  

**Изменил origin**

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img2.1.png)  

**push origin**  

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img2.2.png)  

**Тесты пройдены**

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img2.3.png)  

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img2.4.png)  

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img2.5.png)  

**Но не спервого раза**  

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img2.6.png)  

**Прикладываю код**
```
stages:
  - test
  - static-analysis
  - build

test:
  stage: test
  image: golang:1.17
  script:
   - go test .
sonarqube-check:
 stage: test
 image:
  name: sonarsource/sonar-scanner-cli
  entrypoint: [""]
 variables:
 script:
  - sonar-scanner -Dsonar.projectKey=gitlab -Dsonar.projectName='gitlab' -Dsonar.host.url=http://158.160.136.231:9000  -Dsonar.token=sqp_1185087e8e30f9d20ef75140d2119928e8671195

build:
  stage: build
  image: docker:latest
  script:
   - docker build . 
```
---
## Дополнительные задания* (со звёздочкой)

Их выполнение необязательное и не влияет на получение зачёта по домашнему заданию. Можете их решить, если хотите лучше разобраться в материале.

---

### Задание 3*

Измените CI так, чтобы:

 - этап сборки запускался сразу, не дожидаясь результатов тестов;  

**После регистрации второго ранера с тегом, у меня запумтилось паралельно test и build  

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img3.4.png) 
 
```
stages:
  - test
  - static-analysis
  - build

test:
  stage: test
  image: golang:1.17
  script:
   - go test .
static-analysis:
 stage: test
 image:
  name: sonarsource/sonar-scanner-cli
  entrypoint: [""]
 variables:
 script:
  - sonar-scanner -Dsonar.projectKey=gitlab -Dsonar.projectName='gitlab' -Dsonar.sources=. -Dsonar.host.url=http://51.250.36.218:9000 -Dsonar.login=sqp_1185087e8e30f9d20ef75140d2119928e8671195
  
build:
  stage: test
  image: docker:latest
  script:
   - docker build .
  tags:
    - runner1

```
**Такой вариант тоже сработал, убрал из этапов build и поставил как втрую задачу в test.**  

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img3.5.png)  

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img3.6.png)  

```
stages:
  - test
  - static-analysis

test:
  stage: test
  image: golang:1.17
  script:
   - go test .
build:
  stage: test
  image: docker:latest
  script:
   - docker build .
static-analysis:
 stage: test
 image:
  name: sonarsource/sonar-scanner-cli
  entrypoint: [""]
 variables:
 script:
   - sonar-scanner -Dsonar.projectKey=gitlab -Dsonar.projectName='gitlab' -Dsonar.sources=. -Dsonar.host.url=http://51.250.36.218:9000 -Dsonar.login=sqp_1185087e8e30f9d20ef75140d2119928e8671195
```

**Но есть более продвинутые варианты с расспаралеливанмем процессов с использованием операторов.**

 - тесты запускались только при изменении файлов с расширением *.go  
 
**go test не выполняется если файл** *.go не изменялся.  



```
stages:
  - test
  - static-analysis
  - build

test:
  stage: test
  image: golang:1.17
  script:
   - go test .
  only:
    refs:
      - branches
    changes:
      - "*.go"  
static-analysis:
 stage: static-analysis
 image:
  name: sonarsource/sonar-scanner-cli
  entrypoint: [""]
 variables:
 script:
  - sonar-scanner -Dsonar.projectKey=gitlab -Dsonar.projectName='gitlab' -Dsonar.sources=. -Dsonar.host.url=http://51.250.36.218:9000 -Dsonar.login=sqp_1185087e8e30f9d20ef75140d2119928e8671195

build:
  stage: build
  image: docker:latest
  script:
   - docker build .
     
```

![alt text](https://github.com/travickiy67/gitlab/blob/main/img/img3.7.png)  
