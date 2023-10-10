# Blank function (Go)
![아키텍처](/Images/aws/Lambda/sample-blank-go.png)

- `function` - Golang 함수
- `template.yml` - 애플리케이션을 생성하는 AWS CloudFormation 템플릿
- `1-create-bucket.sh`, `2-deploy.sh` 등 - AWS CLI를 사용하여 애플리케이션을 배포하고 관리하는 셸 스크립트

# 요구사항
- [Go 실행 파일](https://golang.org/dl/).
- Bash 쉘. or [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10) 설치
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) v1.17 이상
AWS CLI v2를 사용하는 경우 [구성 파일](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)(`~/.aws)에 다음을 추가 /구성`):
````
cli_binary_format=raw-in-base64-out
````
이 설정을 사용하면 AWS CLI v2가 v1 동작과 일치하는 파일에서 JSON 이벤트를 로드
# 설정
Download or clone this repository
```
$ git clone https://github.com/awsdocs/aws-lambda-developer-guide.git
$ cd aws-lambda-developer-guide/sample-apps/blank-go
```
To create a new bucket for deployment artifacts, run `1-create-bucket.sh`.
```
blank-go$ ./1-create-bucket.sh
make_bucket: lambda-artifacts-a5e491dbb5b22e0d
```

# 배포
애플리케이션을 배포하려면 '2-deploy.sh'를 실행
```
blank-go$ ./2-deploy.sh
Successfully packaged artifacts and wrote output template to file out.yml.
Waiting for changeset to be created..
Successfully created/updated stack - blank-go
```
이 스크립트는 AWS CloudFormation을 사용하여 Lambda 함수와 IAM 역할을 배포한다. 리소스가 포함된 AWS CloudFormation 스택이 이미 존재하는 경우 스크립트는 템플릿 또는 함수 코드에 대한 변경 사항으로 스택을 업데이트한다.
# 시험
함수를 호출하려면 `3-invoke.sh`를 실행
```
blank-go$ ./3-invoke.sh
{
    "StatusCode": 200,
    "ExecutedVersion": "$LATEST"
}
"{\"FunctionCount\":42,\"TotalCodeSize\":361861771}"
```
스크립트가 함수를 몇 번 호출한 다음 `CRTL+C`를 눌러 종료
애플리케이션은 AWS X-Ray를 사용하여 요청을 추적. [X-Ray 콘솔](https://console.aws.amazon.com/xray/home#/service-map)을 열어 서비스 맵 확인
![서비스 맵](/Images/aws/Lambda/blank-go-servicemap.png)


![추적](/Images/aws/Lambda/blank-go-trace.png)

***
애플리케이션을 삭제하려면 `4-cleanup.sh`를 실행
```
blank$ ./4-cleanup.sh
```