# Cloudify Controls Kubernetes Federation
* Cloudify에서 Kubernetes Federation을 정의하고 deploy 하는 예제입니다.
* Shell 방식 설치를 지원하는 Cloudify Agent
  * https://github.com/kim135797531/cloudify-agent
* Kubernetes Federation 관리를 지원하는 Cloudify Kubernetes Plugin
  * https://github.com/kim135797531/cloudify-kubernetes-plugin
* 예제
  * https://github.com/kim135797531/cloudify-controls-kubernetes-federation

## 준비물
* Cloudify 4.1 Manager
* Cloudify 4.1 CLI
* wagon

### 레포지토리 클론
```bash
git clone https://github.com/kim135797531/cloudify-agent
git clone https://github.com/kim135797531/cloudify-kubernetes-plugin
git clone https://github.com/kim135797531/cloudify-controls-kubernetes-federation
```

## Cloudify Agent
### ShellRunner 지원 설치
* Cloudify 공식 레포지토리에 적용되기 전까지 수동 설치 필요
```bash
cp -rf ./cloudify-agent/cloudify_agent/installer/runners/shell_runner.py /opt/cfy/cloudify-agent/cloudify_agent/installer/runners/shell_runner.py
cp -rf ./cloudify-agent/cloudify_agent/installer/config/agent_config.py /opt/cfy/cloudify-agent/cloudify_agent/installer/config/agent_config.py
cp -rf ./cloudify-agent/cloudify_agent/installer/config/installer_config.py /opt/cfy/cloudify-agent/cloudify_agent/installer/config/installer_config.py
```

## Cloudify Kubernetes 플러그인
### 플러그인 Wagon 빌드 실행
```bash
wagon create -f -s ./cloudify-kubernetes-plugin/
```
### 플러그인 업로드
```bash
cfy plugin upload ./cloudify_kubernetes_plugin-1.2.0-py27-none-linux_x86_64-Ubuntu-trusty.wgn
```

## 예제 블루프린트 실행
### 블루프린트 업로드
```bash
# 블루프린트 default id는 cloudify-controls-kubernetes-federation
cd ./cloudify-controls-kubernetes-federation
cfy blueprints upload ./01-cloudify-agent-for-kubernetes.yaml
```
### 블루프린트 deploy
```bash
# Default deployment id will be set as cloudify-controls-kubernetes-federation
cfy deploy create -b cloudify-controls-kubernetes-federation
```
### Deployment 설치
```bash
cfy executions start install -d cloudify-controls-kubernetes-federation
```

## 예제 삭제
### Deployment, 블루프린트, 플러그인 삭제
```bash
cfy executions start uninstall -d cloudify-controls-kubernetes-federation -f
cfy deployments delete cloudify-controls-kubernetes-federation -f
cfy blueprints delete cloudify-controls-kubernetes-federation
# Check plugin id
PLUGIN_ID=$(cfy plugins list | grep -i cloudify-controls-kubernetes-federation | awk '{print $2;}')
cfy plugins delete $PLUGIN_ID
```
