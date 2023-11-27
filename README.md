### `Helm上`传`Chart`到`Artifact Hub`

##### 创建示例`chart`
首先我们创建一个`helm-charts`文件夹，并且在这个目录下创建一个`helm`示例，并对他进行打包

```shell
helm create helm-sample

helm package helm-sample
```
创建一个charts文件夹，并将打包后的helm-sample-0.1.0.tgz放到该目录下面
```shell
mkdir charts

mv helm-sample-0.1.0.tgz charts
```

##### 创建`Index`索引文件

索引文件是一个名为 `index.yaml` 的 `Yaml` 文件。它包含有关 `Chart` 包的一些元数据，包括 `Chart.yaml` 文件的内容。有效的 `Chart Repository` 必须具有索引文件。索引文件包含有关 `Chart Repository` 中每个 `Chart` 的信息。可以通过 `helm repo index` 命令将本地目录下的 `Chart` 生成索引文件。

`helm repo index .`

然后简单修改一下里面的信息：
```yaml
apiVersion: v1
entries:
  helm-sample:
  - apiVersion: v2
    appVersion: 1.16.0
    created: "2023-11-27T11:32:08.795143+08:00"
    description: A Helm chart for Kubernetes
    digest: 1ba6d827b6a269434b821a6fa13f52b9a9ce087714e1ce8624b428f2e38aff4c
    name: helm-sample
    type: application
    version: 0.1.0
    home: https://lucas234.github.io/helm-charts/
    kubeVersion: ^1.10.0-0
    keywords:
    - helm-sample
    - elastic
    - vector
    - search
    - deploy
    maintainers:
    - email: ly_liubo@163.com
      name: lucas
      url: https://lucas234.github.io/helm-charts/
    sources:
    - https://github.com/lucas234/helm-charts
    urls:
    - https://github.com/lucas234/helm-charts/blob/main/charts/helm-sample-0.1.0.tgz
generated: "2023-11-27T11:32:08.793495+08:00"
```

最后由于`CHART REPOSITORY`标准，我们需要将`index.yaml`放到`charts`下面:

`cp index.yaml charts`

##### 创建`github`仓库
首先我们创建一个github仓库：https://github.com/lucas234/helm-charts
然后上传项目到我们的github仓库中。
```git
git init
git add .
git commit -m "add files"
git branch -M main
git remote add origin https://github.com/lucas234/helm-charts
git push origin main
```

##### 创建`WEB`服务
这里通过`GitHub Pages`为`Chart Repository`提供`Web`服务。
GitHub Action 允许你以两种不同的方式提供静态网页：
1. 通过配置项目以提供其`docs/`目录的内容
2. 通过配置项目以提供特定分支的内容

我们将采用第二种方法，尽管第一种方法同样简单。
第一步是创建 helm-sample-pages 分支:

```
git checkout -b helm-sample-pages
git push origin helm-sample-pages
```

接下来，你将要确保将`helm-sample-pages`分支设置为`GitHub Pages`，点击你的`repo` `Settings`并点击到`Pages`，选择`branch`后点击`Save`,即可在Github pages下边看到
`Your site is live at https://lucas234.github.io/helm-charts/`


然后我们打开Artifact Hub的仓库地址:https://artifacthub.io/control-panel/repositories
并进行添加我们的helm-sample, URL选项填入`https://lucas234.github.io/helm-charts/` 即可
