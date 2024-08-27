---
title: "AWS EC2에서 Ansible을 사용하여 Jenkins를 배포하는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-SimplestepsindeployingJenkinsonAWSEC2usingAnsible_0.png"
date: 2024-08-26 18:59
ogImage: 
  url: /assets/img/2024-08-26-SimplestepsindeployingJenkinsonAWSEC2usingAnsible_0.png
tag: Tech
originalTitle: "Simple steps in deploying Jenkins on AWS EC2 using Ansible"
link: "https://medium.com/@vinuthan2/simple-steps-in-deploying-jenkins-on-aws-ec2-using-ansible-ab9f598e6d67"
isUpdated: true
updatedAt: 1724742657409
---


이 문서에서는 AWS EC2 인스턴스를 생성하고, Amazon Linux 머신에 Jenkins를 설치하기 위한 Ansible 플레이북을 작성할 것입니다.

Jenkins는 지속적 통합 및 지속적 배포(CI/CD) 파이프라인에 사용할 수 있는 강력한 자동화 서버입니다.

Ansible은 인프라의 프로비저닝과 구성을 자동화하는 데 사용되는 인기 있는 구성 관리 도구입니다.

필수 사항:
Ansible과 Jenkins의 기본 지식과 다음의 필수 사항이 있는지 확인해주세요:

<div class="content-ad"></div>

- AWS 계정
- Amazon Linux 인스턴스에 대한 EC2 인스턴스 연결 또는 SSH 액세스

# 단계 1: Amazon EC2 인스턴스 시작하기

EC2 인스턴스를 시작하려면:

- AWS Management Console에 로그인합니다.
- 계산 아래에서 EC2을 선택하여 Amazon EC2 콘솔을 엽니다.
- Amazon EC2 대시보드에서 '인스턴스 시작'을 선택합니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-26-SimplestepsindeployingJenkinsonAWSEC2usingAnsible_0.png)

4. Choose an OS Images Amazon Machine Image (AMI) Free tier eligible Amazon Linux 2023 AMI

![이미지](/assets/img/2024-08-26-SimplestepsindeployingJenkinsonAWSEC2usingAnsible_1.png)

5. Scroll down and create a new key pair for SSH connection. For simplicity and quick access, we will use EC2 Instance connect.


<div class="content-ad"></div>

6. 새 보안 그룹을 만들고 다음 규칙을 추가하세요:

- 어디서나 들어오는 HTTP 액세스를 허용합니다.
- 내 컴퓨터의 공개 IP 주소에서 들어오는 SSH 트래픽을 허용하여 인스턴스에 연결할 수 있도록 합니다.
- 포트 범위에 8080을 입력합니다.

7. 인스턴스 시작을 선택하세요.

![이미지](/assets/img/2024-08-26-SimplestepsindeployingJenkinsonAWSEC2usingAnsible_2.png)

<div class="content-ad"></div>

8. 왼쪽 네비게이션 바에서 '인스턴스'를 선택하여 인스턴스의 상태를 확인하세요. 처음에는 인스턴스 상태가 대기 중인 'pending' 상태일 것입니다. 상태가 'running'으로 변경되면 인스턴스를 사용할 준비가 된 것입니다.

![인스턴스 상태 이미지](/assets/img/2024-08-26-SimplestepsindeployingJenkinsonAWSEC2usingAnsible_3.png)

9. Linux 인스턴스에 연결하기

- EC2 인스턴스 연결을 사용하여 연결하세요.

<div class="content-ad"></div>


![Step 2](/assets/img/2024-08-26-SimplestepsindeployingJenkinsonAWSEC2usingAnsible_4.png)

![Step 3](/assets/img/2024-08-26-SimplestepsindeployingJenkinsonAWSEC2usingAnsible_5.png)

![Step 4](/assets/img/2024-08-26-SimplestepsindeployingJenkinsonAWSEC2usingAnsible_6.png)

# Step 2: Install Ansible


<div class="content-ad"></div>

- EC2 인스턴스에 연결하고, root 사용자로 전환하여 ansible을 설치해 보세요:

```js
sudo -i
sudo yum update && yum install ansible
```  

![이미지](/assets/img/2024-08-26-SimplestepsindeployingJenkinsonAWSEC2usingAnsible_7.png)

2. Ansible 버전 확인하기

<div class="content-ad"></div>

```js
앤서블 --버전
```

<img src="/assets/img/2024-08-26-SimplestepsindeployingJenkinsonAWSEC2usingAnsible_8.png" />

3. 앤서블 플레이북을 사용하여 젠킨스를 설치할 것입니다.

- 앤서블 플레이북 yaml 파일을 생성할 것입니다: setup_jenkins.yaml

<div class="content-ad"></div>

```yaml
cat > setup_jenkins.yml << EOF 
---
- hosts: localhost
  connection: local
  vars:
    java_packages:
      - java-17-amazon-corretto-devel
    jenkins_packages:
      - jenkins  
  tasks:
    - name: Download Jenkins repository file
      get_url:
        url: https://pkg.jenkins.io/redhat-stable/jenkins.repo
        dest: /etc/yum.repos.d/jenkins.repo
      become: true

    - name: Import Jenkins-CI key
      shell: rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
      become: true

    - name: Install Java
      yum:
        name: "{ java_packages }"
        state: present
      become: true

    - name: Install jenkins
      yum:
        name: "{ jenkins_packages }"
        state: present
      become: true

    - name: Start jenkins service
      service:
        name: jenkins
        state: started
      become: true
EOF
```

4. 그런 다음 실행하십시오:

```js
ansible-playbook setup_jenkins.yml
```

5. 성공하면 아래 출력을 확인할 수 있어야 합니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-26-SimplestepsindeployingJenkinsonAWSEC2usingAnsible_9.png)

6. 이제 브라우저에서 공용 IPv4 주소를 사용하여 포트 8080으로 이동합니다.

예시: http://3.6.86.139:8080/

![이미지](/assets/img/2024-08-26-SimplestepsindeployingJenkinsonAWSEC2usingAnsible_10.png)


<div class="content-ad"></div>

7. 브라우저에서 Jenkins가 시작되었을 때, 아래와 같이 패스워드를 찾을 수 있습니다:

![Jenkins Password](/assets/img/2024-08-26-SimplestepsindeployingJenkinsonAWSEC2usingAnsible_11.png)

8. 패스워드를 얻으려면:

```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

<div class="content-ad"></div>

9. Jenkins 잠금 해제: 화면 안내에 따라 Jenkins를 잠금 해제하고 초기 설정을 완료하세요.

10. Jenkins 구성: 필요에 따라 Jenkins 설정을 사용자 정의하고 플러그인을 설치하여 요구 사항에 맞게 설정하세요.

![이미지](/assets/img/2024-08-26-SimplestepsindeployingJenkinsonAWSEC2usingAnsible_12.png)

이 프로젝트가 마음에 드셨으면 좋겣습니다. 좋아요를 남겨주시고 다음 프로젝트에서 만나요!