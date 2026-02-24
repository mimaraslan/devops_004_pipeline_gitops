# AWS

### DevOps - Local
![DevOps Local.jpg](img/DevOps%20Local.jpg)

### DevOps - Cloud
![DevOps Cloud.jpg](img/DevOps%20Cloud.jpg)

####  AWS kaynaklarının ARN standart adres yolları

```
arn:aws:resource-groups:us-east-1:1111111:group/Arge_Departmani_Kaynaklari

arn:aws:account        :          :1111111:account

arn:aws:iam:           :1111111:user/AbdussamedBey
```

AWS Management Console Giriş Adresleri

AWS_ID_NUMARAN : 1111 2222 3333

https://111122223333.signin.aws.amazon.com/console

---
AWS ID yerine oluşturduğun bir FIRMA ADI ile giriş adresi

FIRMA ADI : MurlindaSoft

https://MurlindaSoft.signin.aws.amazon.com/console

---
Parola kuralımız

1 büyük harf

1 küçük harf

1 sayı

1 simge

```
Adana_01
Ankara_06
Antalya_07
Izmir_35
Istanbul_34
Trabzon_61
Agri_004
```


---
Amazon CLI - Setup

https://aws.amazon.com/cli/

---
AWS CLI Command Reference

https://docs.aws.amazon.com/cli/latest/

---

Termimalden AWS'ye erişmek için Access key oluşturacağız.

Kullanıcı adı ->  Security credentials -> Create access key

Adres burası
https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#security_credential

---

Access key
AAAAAAAAAAA

Secret access key
BBBBBBBBBBBB

---
Terminali aç.

```
aws --version
```
```
aws configure
```
```
AWS Access Key ID [****************]: AAAAAAAAAAAAA
AWS Secret Access Key [****************]: BBBBBBBBB
Default region name [us-east-1]: us-east-1
Default output format [None]: json
```

```
aws ec2 describe-instances
```

---

PuTTY
https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html



==============================





===== Makine 1 : My Jenkins Master =====

Public IP numaralarını Elastic IP ile mutlaka sabitliyoruz!!!

https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Addresses:





MacOS üzerinden terminalden bağlantı için instances makineyi seç.
Connect düğmesine bas.
SSH client sekmesini aç.

Terminalden  My-Ubuntu-Key.pem konumuna   git.
```
chmod 400 "My-Ubuntu-Key.pem"
```

```
ssh -i "My-Ubuntu-Key.pem" ubuntu@ec2-PUBLIC_IP.compute-1.amazonaws.com
```





Windows üzerinden MobaXterm ile SSH bağlantısını kurduk.



Makineyi güncelliyorum.

```
sudo apt update

sudo apt upgrade -y
```

İç IP yerine makineye bir isim veriyoruz.

```
sudo nano /etc/hostname
```

Makinememizin adı: My-Jenkins-Master

Ctrl + X'e bas.
Onaylamak için Y tuşuna bas.
En sonda da Enter'a bas.

Makineyi yeniden başlatacağız.

```
sudo reboot
```
Ya da

```
sudo init 6
```




===========================

EC2 -> Security Groups -> Inbound rules  -> Edit inbound rules
8080 portunu erişime açıyoruz.

======== Java'yı kuracağız. ===================

Terminale sadece "java" yaz ve enter tuşuna bas. Açılan komutlardan 21. sürümü al.
```
sudo apt install openjdk-21-jre-headless -y

java --version
```

======== Jenkins kuracağız. ===================

https://www.jenkins.io/doc/book/installing/linux/

```
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian/jenkins.io-2026.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
https://pkg.jenkins.io/debian binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins   -y
```

```
sudo systemctl enable jenkins

sudo systemctl start jenkins

sudo systemctl status jenkins
```



http://PUBLIC_IP:8080/login

Bu adresin içindeki yazıyı terminalde göster.
sudo cat  /var/lib/jenkins/secrets/initialAdminPassword


Varsayılan kurulumları yap. O arada Git aracı da makineye otomatik olarak kurulacak.


http://PUBLIC_IP:8080/manage/pluginManager/available

Pipeline: Stage View bu eklentiyi kur.











===== Makine 2 : My Jenkins Agent =====


Public IP numaralarını Elastic IP ile mutlaka sabitliyoruz!!!

https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Addresses:





MacOS üzerinden terminalden bağlantı için instances makineyi seç.
Connect düğmesine bas.
SSH client sekmesini aç.

Terminalden  My-Ubuntu-Key.pem konumuna   git.
```
chmod 400 "My-Ubuntu-Key.pem"

ssh -i "My-Ubuntu-Key.pem" ubuntu@ec2-PUBLIC_IP.compute-1.amazonaws.com
```



Windows üzerinden MobaXterm ile SSH bağlantısını kurduk.



Makineyi güncelliyorum.
```
sudo apt update

sudo apt upgrade -y
```

İç IP yerine makineye bir isim veriyoruz.
```
sudo nano /etc/hostname
```

Makinememizin adı: My-Jenkins-Agent

Ctrl + X'e bas.
Onaylamak için Y tuşuna bas.
En sonda da Enter'a bas.

Makineyi yeniden başlatacağız.

```
sudo reboot
```


======== Java'yı kuracağız. ===================

Terminale sadece "java" yaz ve enter tuşuna bas. Açılan komutlardan 21. sürümü al.

```
sudo apt install openjdk-21-jre-headless -y

java --version
```



======== Docker'ı kuracağız. ===================

Terminale sadece "docker" yaz ve enter tuşuna bas.

Açılan komutlardan yararlanacağız.
```
sudo apt  install docker.io  -y
```

Grup adını ekliyoruz.
```
sudo groupadd docker
```

```
sudo usermod -a -G docker $USER
```
veya

```
sudo usermod -aG docker $USER
```

Yeniden başlat.
```
sudo reboot
```
================================

Makineleri birbirine tanıtacağız.

=== My Jenkins Agent için ===
```
sudo nano /etc/ssh/sshd_config
```

Şu 2 satırın önündeki # işaretini kaldırıyorum.

PubkeyAuthentication yes

AuthorizedKeysFile      .ssh/authorized_keys .ssh/authorized_keys2


Ctrl + X'e bas.
Onaylamak için Y tuşuna bas.
En sonda da Enter'a bas.


=== My Jenkins Master için ===
```
sudo nano /etc/ssh/sshd_config
```

### Authentication:
Authentication kısmına gel.
Aşağıdaki şu iki satırın önündeki açıklama işaretini # kaldır.


PubkeyAuthentication yes

AuthorizedKeysFile      .ssh/authorized_keys .ssh/authorized_keys2


Ctrl + X'e bas.
Onaylamak için Y tuşuna bas.
En sonda da Enter'a bas.


=== My Jenkins Master için ===

```
sudo service ssh reload
```
veya
```
sudo systemctl reload ssh
```


=== My Jenkins Master için ===

```
sudo service ssh reload
```
veya
```
sudo systemctl reload ssh
```


=== My Jenkins Master için ============================

Konumumuz  /home/ubuntu   olacak.

pwd konumu bize gösterir.

```
pwd

cd /home/ubuntu

ssh-keygen
```


Master makinenin takip edilebilmesi için bir şifre anahtar oluşturuyorum.

```
cd /home/ubuntu/.ssh/

ll

sudo cat  id_ed25519.pub
```


İçindeki böyle yazan satırı alıp kopyalayın.

```
ssh-ed25519 AAAAAAAAAAAAAAAAA ubuntu@My-Jenkins-Master
```


=== My Jenkins Agent için ============================
```
cd /home/ubuntu/.ssh/

ll

sudo cat authorized_keys
```

Bu dosyanın için aç. Agent Takip eden taraftır.

```
ssh-rsa BBBBBBBBBBBBBBBBBB MyAWSKeyPair
```

```
sudo nano authorized_keys
```


Master'dan aldığın şu satırı en alta yapıştır.
```
ssh-ed25519 AAAAAAAAAAAAAAAAA ubuntu@My-Jenkins-Master

```

Master ve Agent makinelerini yeniden başlat.
```
sudo reboot
```

======================================
Master makinedeki Jenkins'i aç.

http://PUBLIC_ID:8080/computer/(built-in)/configure

Jenkins'i aç. Nodes kısmına gel.

Built-In Node makinesinin içine gir.

Nodes -> Built-In Node -> Configure

Number of executors kısmını SIFIR 0 yap.

Agent makineyi Jenkins'e eklemek için

Nodes -> New node

http://PUBLIC_IP:8080/computer/new

Ona "My-Jenkins-Agent" diye bir isim verdik. Permanent Agent olduğunu seçtik.

Jenkins'te Agent'ı eklerken Agent'ın kendi private özel yani iç IP'sini alacaksın.


====== Master Makinedeki bu anahtarı okuyup kopyalayın ve Jenkins'e gelin. Credentials için ====

```
cd  /home/ubuntu/.ssh/
sudo cat id_ed25519
```

```
-----BEGIN OPENSSH PRIVATE KEY-----
CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC
-----END OPENSSH PRIVATE KEY-----
```







===== GitHub Token ======


MyGitHubTokenAWS
ghp_AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA

Jenkins'e tanıttık.

==============================



===== Makine 3:  SonarQube Sunucusu ========

Public IP numaralarını Elastic IP ile mutlaka sabitliyoruz!!!

https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Addresses:





MacOS üzerinden terminalden bağlantı için instances makineyi seç.
Connect düğmesine bas.
SSH client sekmesini aç.

Terminalden  My-Ubuntu-Key.pem konumuna   git.
```
chmod 400 "My-Ubuntu-Key.pem"

ssh -i "My-Ubuntu-Key.pem" ubuntu@ec2-PUBLIC_IP.compute-1.amazonaws.com
```



Windows üzerinden MobaXterm ile SSH bağlantısını kurduk.



Makineyi güncelliyorum.
```
sudo apt update

sudo apt upgrade -y
```

İç IP yerine makineye bir isim veriyoruz.
```
sudo nano /etc/hostname
```

Makinememizin adı: My-SonarQube

Ctrl + X'e bas.
Onaylamak için Y tuşuna bas.
En sonda da Enter'a bas.

Makineyi yeniden başlatacağız.
```
sudo reboot
```

Makineyi yeniden başlatmadan bu komutu da kullanabiliriz.
```
sudo hostnamectl set-hostname My-SonarQube --static
```





======== Java'yı kuracağız. ===================

Terminale sadece "java" yaz ve enter tuşuna bas. Açılan komutlardan 21. sürümü al.
```
sudo apt install openjdk-21-jre-headless -y

java --version
```







====  PostgreSQL kurulumu  =====
```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

wget -qO- https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/pgdg.asc &>/dev/null



sudo apt update

sudo apt-get -y install postgresql postgresql-contrib



sudo systemctl enable postgresql



sudo passwd postgres
```
parola: 123456789




```
su - postgres
```
parola: 123456789
```
createuser sonar

psql

ALTER USER sonar WITH ENCRYPTED password 'sonar';

CREATE DATABASE sonarqube OWNER sonar;


GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonar;

veya

grant all privileges on DATABASE sonarqube to sonar;


\q

exit
```







Burada bunu kurmayacağız. İhtitaç durumuda Ubuntu'ya repolar ekleriz.
==== Adoptium repository ====
```
sudo bash

wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc

echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list



sudo apt update


sudo apt install openjdk-17-jre -y
```
OR
```
sudo apt install temurin-17-jdk -y


sudo update-alternatives --config java

java --version
```




=== Linux kernel  ===

```
sudo nano /etc/security/limits.conf
```
veya
```
sudo vim /etc/security/limits.conf
```
Bir şey eklemek için önce klavyeden i tuşuna bas.
```
sonarqube   -   nofile   65536
sonarqube   -   nproc    4096
```

çıkış için ESC tuşuna bas.
```
:wq  yaz
```




```
sudo vim /etc/sysctl.conf
```
Bir şey eklemek için önce klavyeden i tuşuna bas.
Eklenecek bilgi aşağıdaki satır.
```
vm.max_map_count = 262144
```

Çıkış için ESC tuşuna bas.
:wq  yaz


Makineyi yeniden başlat.

sudo init 6

OR
```
sudo reboot
```





==== Sonarqube kurulumu  =====
```
pwd

cd /opt

sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-26.2.0.119303.zip


sudo apt install unzip


sudo unzip sonarqube-26.2.0.119303.zip -d/opt

pwd

sudo mv   /opt/sonarqube-26.2.0.119303    /opt/sonarqube
```



sonar kullanıcı oluşturulacak ve haklar verilecek

```
sudo groupadd sonar

sudo useradd -c "user to run SonarQube" -d /opt/sonarqube -g sonar sonar

sudo chown sonar:sonar /opt/sonarqube -R
```


veritabanıyla bu kullanıcıyı konuştur
```
sudo vim /opt/sonarqube/conf/sonar.properties
```

```
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar


sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube
```







Sonar servisini oluşturacağız.

```
sudo vim /etc/systemd/system/sonar.service
```

Aşağıdaki kodları olduğu gibi bu dosyanın içine yapıştır.


```
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking

ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always

LimitNOFILE=65536
LimitNPROC=4096

[Install]
WantedBy=multi-user.target
```



Makine açıldığında sonarqube otomatik olarak çalıştırma komutları

```
sudo systemctl enable sonar

sudo systemctl start sonar

sudo systemctl status sonar
```




=== Log takibi ===
```
sudo tail -f /opt/sonarqube/logs/sonar.log
```

Makinenin public ip değerini al ve 9000 portundan giriş yap.
kullanıcı: admin
parola: admin


Yeni parola: Adana_01Adana_01

Jenkins içinde tokenı kaydettir.

Pluginleri kur.

Sonar'ın kurulduğu makinenin Private IPv4 addresses değerini kopayla.

http://PUBLIC_IP:9000





### === Docker ====
Docker'ı Jenkins'e tanıttık.

DockerHub'a gidip Token oluşturuyoruz.


https://hub.docker.com/

Personal access tokens

MyDockerHubTokenAWS

dckr_pat_DDDDDDDDDDDDDDDDDDDDDDD


```
docker login -u mimaraslan  -p  dckr_pat_DDDDDDDDDDDDDDDDDDDDDDD
```




### ===== Makine 4 : My EKS Server =====


Public IP numaralarını Elastic IP ile mutlaka sabitliyoruz!!!

https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Addresses:



MacOS üzerinden terminalden bağlantı için instances makineyi seç.
Connect düğmesine bas.
SSH client sekmesini aç.

Terminalden  My-Ubuntu-Key.pem konumuna   git.

```
chmod 400 "My-Ubuntu-Key.pem"
```

```
ssh -i "My-Ubuntu-Key.pem" ubuntu@ec2-PUBLIC_IP.compute-1.amazonaws.com
```


Windows üzerinden MobaXterm ile SSH bağlantısını kurduk.



Makineyi güncelliyorum.

```
sudo apt update

sudo apt upgrade -y
```

İç IP yerine makineye bir isim veriyoruz.

```
sudo nano /etc/hostname
```

Makinememizin adı: My-EKS-Server

Ctrl + X'e bas.

Onaylamak için Y tuşuna bas.

En sonda da Enter'a bas.


Makineyi yeniden başlatacağız.

```
sudo reboot
```



Bu adrese gidiyoruz. AWS CLI'yı bu makineye kuracağım.



https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

```
sudo apt install unzip

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

aws --version
```




Python'u da kurduk.
```
sudo apt install python3-pip -y
```


Kubernetes ile konuşabilmemiz için kubectl kurmamız gerekiyor.

https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html


```
sudo su

curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.33.5/2025-11-13/bin/linux/amd64/kubectl

ll


chmod +x ./kubectl

ll


mv kubectl /bin


kubectl version --output=yaml
```




### eksctl kurulumu

Web Sayfası

https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html#linux_amd64_kubectl
```
sudo su

pwd

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C   /tmp

cd /tmp

ll

sudo   mv    /tmp/eksctl     /bin

eksctl version
```



```
sudo reboot
```


```
kubectl version --client
```




---------------------------------

Önce makineye Admin yetkisi verdik.
https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/roles

## EKS kurmak
```
eksctl create cluster --name my-workspace-cluster \
--region us-east-1 \
--node-type c5ad.large \
--nodes 2 
```


```
sudo reboot
```


```
kubectl get nodes

kubectl get pods
```





### ArgoCD


Web Sayfası
https://argo-cd.readthedocs.io/en/stable/getting_started/

```
kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl get pods -n argocd
```


Yönetici moduna geç.
```
sudo su

pwd
```



ArgoCD sürümünü çekeceğiz.


Son sürümü çekmek için bu 3 satır gerekli.

```
curl -L -s https://raw.githubusercontent.com/argoproj/argo-cd/stable/VERSION

VERSION=$(curl -L -s https://raw.githubusercontent.com/argoproj/argo-cd/stable/VERSION)

curl --silent --location -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/v$VERSION/argocd-linux-amd64
```


Duruma göre berlirli bir sürüme ihtiyacınız olursa bunu kullanın. Yukarıda 3 komut en güncel sürümü çeker.
```
curl --silent --location -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/v2.12.3/argocd-linux-amd64
```

Çalıştırma hakkını aldım.
```
chmod +x /usr/local/bin/argocd
```


Normal kullanıcı moduna geç. Terminale bunu yaz.
```
exit
```

```
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

kubectl get svc -n argocd
```

ArgoCD dış dünyaya böyle bir adres ile açılıyor.

a5b3d196d6343444dbd692184429ca6b-117814533.us-east-1.elb.amazonaws.com







### EKS nodelarını silme

https://docs.aws.amazon.com/eks/latest/userguide/delete-cluster.html
```
eksctl version
```
Çalışan tüm servisleri listele
```
kubectl get svc --all-namespaces
```

Sadece bir servisi silmek için bu komutu kullanıyoruz ama servisleri tek tek silmeyeceğiz.
```
kubectl delete svc SERVISIN_ADI
```


Cluster adını ve detaylarını görmek için
```
kubectl config view
```

Silmeden önce konumu belirtmek lazım.
```
export AWS_DEFAULT_REGION=us-east-2
```

Nodeları ve servislerin hepsini cluster adını vererek toplu halde sileceğiz.
```
eksctl delete cluster --name my-workspace2-cluster
```





 