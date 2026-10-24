# Dify 工作流演示

本目录包含“通用简历初筛｜按岗位要求审核”的三类结果版工作流和“寻味入山｜茶叶营销文案审核”工作流。两者均为课堂辅助演示，输出供人工复核。简历工具不自动执行录用或淘汰操作，茶叶工具不自动发布文案。

## 当前状态

本仓库提供工作流 DSL 和 API 请求示例；尚未部署公网，也未验证公开 API 调用成功。上传本目录不会自动生成可运行的 Dify 后端。

## 自行导入使用

在自己的 Dify 工作区导入 resume-screening.yml 或 tea-copy-review.yml，按 Dify 提示安装所声明的 DeepSeek 插件，配置自己的模型凭证，核对模型可用性并发布工作流。导出来源为本机 Dify 1.17.1，DSL 版本为 0.7.0；其他版本须实际验证兼容性。

## 调用已部署的 API

在提供方部署并发布工作流、私下分配调用权限后，将实际服务地址设置为 DIFY_API_BASE（以 /v1 结尾），将应用调用密钥设置为 DIFY_APP_KEY。这里使用的是 Dify 应用密钥，不是模型供应商密钥。密钥不可放进网页前端或 GitHub。

```sh
curl --fail-with-body --request POST "${DIFY_API_BASE}/workflows/run" \
  --header "Authorization: Bearer ${DIFY_APP_KEY}" \
  --header "Content-Type: application/json" \
  --data-binary @resume-request.json
```

输入字段为 jd 和 resume。示例均为仿写，请勿上传真实简历作为公开测试文件。工作流有多个结束分支，调用方应按实际返回的 data.outputs 处理；不要假定各分支字段名相同。

## 对外提供服务前

确定公网 Dify 或服务端代理地址，配置访问控制、用量限制和调用费用承担方式，再完成真实连通测试。不要把本机管理后台整体开放给公众。若希望每个使用者有独立权限与配额，应在服务端代理层实现，不应把同一应用密钥写到公开仓库。

## 参考

Dify 官方 API 实现：https://github.com/langgenius/dify/blob/main/api/controllers/service_api/app/workflow.py
GitHub Pages 是静态托管：https://docs.github.com/en/pages/getting-started-with-github-pages/what-is-github-pages

## 茶叶营销文案审核

输入字段为 product_name（产品名）、selling_point（有依据的卖点）、audience（目标受众）、stage（营销阶段，只能填写“认知”“兴趣”“购买”“忠诚”之一）。调用时将上面的请求文件改为 tea-request.json，并使用茶叶工作流自己的应用密钥。输出随审核分支不同，读取实际 data.outputs。
