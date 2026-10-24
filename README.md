# Dify 工作流演示

本目录包含“通用简历初筛｜按岗位要求审核”的三类结果版工作流和“寻味入山｜茶叶营销文案审核”工作流。两者均为课堂辅助演示，输出供人工复核。简历工具不自动执行录用或淘汰操作，茶叶工具不自动发布文案。

## 当前状态

本仓库提供工作流 DSL 和 API 请求示例。两套工具均已在 Dify Cloud 发布，运行服务由 Dify Cloud 提供。

[通用简历初筛｜按岗位要求审核](https://udify.app/workflow/p3YhyWfe7bY9oWvJ) · [管理后台（需登录）](https://cloud.dify.ai/app/f612d884-1728-45a5-8626-6e8f7ea4e6fe/access-point)

[寻味入山｜茶叶营销文案审核](https://udify.app/workflow/DfbbldzveyaQErTy) · [管理后台（需登录）](https://cloud.dify.ai/app/f42dd4ca-8ca7-4585-b62d-3408e3fe820c/access-point)

两套工具均已用虚构样例完成云端内部测试。两个公开网页入口均返回 HTTP 200，未跳转登录；另已在 Safari 分别提交一次虚构样例，均显示工作流运行成功并返回结果：茶叶工具“审核通过”，简历工具“本次不进入面试”。

两套工具已各创建 1 个独立应用 API 密钥，并使用仓库中的两份虚构请求示例完成真实 API 调用：均返回 HTTP 200，运行状态为 succeeded。密钥不公开，需向所有者申请调用权限。

## 自行导入使用

在自己的 Dify 工作区导入 resume-screening.yml 或 tea-copy-review.yml，按 Dify 提示安装所声明的 DeepSeek 插件，配置自己的模型凭证，核对模型可用性并发布工作流。导出来源为本机 Dify 1.17.1，DSL 版本为 0.7.0；其他版本须实际验证兼容性。

## 调用已部署的 API

取得所调用工具的应用密钥后，将 DIFY_API_BASE 设置为 https://api.dify.ai/v1，将应用调用密钥设置为 DIFY_APP_KEY。这里使用的是 Dify 应用密钥，不是模型供应商密钥。密钥不可放进网页前端或 GitHub。

```sh
curl --fail-with-body --request POST "${DIFY_API_BASE}/workflows/run" \
  --header "Authorization: Bearer ${DIFY_APP_KEY}" \
  --header "Content-Type: application/json" \
  --data-binary @resume-request.json
```

输入字段为 jd 和 resume。示例均为仿写，请勿上传真实简历作为公开测试文件。工作流有多个结束分支，调用方应按实际返回的 data.outputs 处理；不要假定各分支字段名相同。

## 调用注意

调用前与所有者确认权限、用量和费用。若需要每位使用者有独立权限与配额，可通过服务端代理实现。

## 参考

Dify 官方 API 实现：https://github.com/langgenius/dify/blob/main/api/controllers/service_api/app/workflow.py
GitHub Pages 是静态托管：https://docs.github.com/en/pages/getting-started-with-github-pages/what-is-github-pages

## 茶叶营销文案审核

输入字段为 product_name（产品名）、selling_point（有依据的卖点）、audience（目标受众）、stage（营销阶段，只能填写“认知”“兴趣”“购买”“忠诚”之一）。调用时将上面的请求文件改为 tea-request.json，并使用茶叶工作流自己的应用密钥。输出随审核分支不同，读取实际 data.outputs。
