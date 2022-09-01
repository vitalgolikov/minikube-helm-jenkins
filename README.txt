# Vargant environment
  В качестве сценария, который поднимает окружение kubernetes был подобран сценарий с репозитария  https://github.com/kmitsolution/k8s
  Так как обладаю в качесве рабочей машины ноутом, уменьшил количество рабочих нод до 1 ( NodeCount = 1 ). Данная ситуация не спасла и сделала мой
  рабочий ноут "задумчивый".  Получилось разве что авторизоваться
  vagrant ssh kmaster

  Было принято решение проделать делаее задание на minikube


# Подготовка среды и развертывание pod с jenkins
   1) Подготовленный файл с описанием namestape
      kubectl apply -f jenkins-namespace.yaml
   2) Подготовлен файл с описание PV для Jenkins
      kubectl apply -f jenkins-volume.yaml
   3) Создан сервис аккаунт
      kubectl apply -f jenkins-sa.yaml

   4) Просмотр и экспорт текущих значений helm chart
       helm show values jenkins/jenkins
       helm show values jenkins/jenkins > values.yaml
   5) Замена ряда параметров на предопределенные
       change values persistence:
       change values serviceAccount:
       для миникуба выбран ServiceType  NodePort
   6) Деплой Jenkins
       helm install jenkins -n jenkins -f values.yaml jenkins/jenkins

# Траблшутинг
    Под не захотел подниматься вынужден был глянуть в его логи
    kubectl logs -n jenkins jenkins-0 init
    Оказалось, что нет доступа  в PV , нашел на stack overflow решение
    minikube ssh
    sudo chown -R 1000:1000 /data/jenkins-volume


# Для получениея URL и admin ключа подкинул сценарии
     show_admin_key.sh
     show_url.sh

# Работа с UI
  Вынужден был достаить плагин Docker
  Подключил slave агент
  Установил ему метку, выполнил прсотой  pieplan на однин стейдж

