# Service EKS Cluster 생성용 IaC Repository

ⓘ 목적 : Service EKS Cluster Provisioning 하기위해 IaC를 도입 및 운영에 활용한다.

## IaC Terraform 수행
 - IaC Code 수행내역    
   Service VPC 및 Subnet, Route Table, Nat/Internet Gateway, EIP 생성 및 설정    
   Service EKS Cluster / Service EKS Cluster Node Group 생성 및 설정    
      
<br>

### 1. AWS Region 설정한다.

- us-west-2(Oregon) 리젼에서 진행
- terraform.tfvars

```bash
aws_region = "us-west-2" # service VPC가 생성될 region 설정        
```


### 2. Terraform Backend(S3) 설정한다. (예시이므로 반드시 본인 정보로 설정)

- S3 버킷은 us-east-1(N.Virginia) 리젼에서 생성
- provider.tf 수정

```bash
terraform {
  backend "s3" {
    bucket = "<<생성한 S3 Bucket 명>>"    # 생성한 s3 bucket
    key    = "eshop/terraform.tfstate"
    region = "us-east-1"      # s3가 생성된 region us-east-1
  }
  required_version = ">=1.1.3"
}
provider "aws" {
  region = var.aws_region
}
```

<br>

### 3. 사전 Jenkins awsCredentials 세팅

- 반드시 ID값을 'awsCredentials'로 추가
- `Add Credentials` 클릭 후 AWS credentials 생성한다.
- jenkins pipeline 에서 terraform 을 이용해 eshop용 인프라 생성을 한다. 이를 위해 AWS IAM 사용자의 access key를 등록

> |항목|내용|액션|
> |---|---|---|
> |➕ Kind | `AWS Credentials` | 선택|
> |➕ ID |  `awsCredentials` | 입력|
> |➕ Access Key ID |  << AWS Access Key ID >> | 📌 메모값 입력|
> |➕ Secret Access Key  | << AWS Secret Access key >> | 📌 메모값 입력|

<br>
<br>