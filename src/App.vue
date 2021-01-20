<template>
  <h1>
    我是Drone自动构建的 gh-pages 页面
  </h1>
  <pre>
kind: pipeline
type: docker
name: default

clone:
  depth: 1

steps:
- name: release
  image: registry.cn-beijing.aliyuncs.com/wa/node
  environment:
    GITHUB_TOKEN:
      from_secret: GITHUB_TOKEN
  when:
    event: push
    branch: master
  commands:
    - echo "branch is $DRONE_BRANCH, commit id is $DRONE_COMMIT"
    - rm -rf './dist'
    - npm install
    - npm run build
    - cd ./dist
    - git init
    - git checkout -b gh-pages
    - git add .
    - git commit -m 'drone update file'
    - git push "https://$GITHUB_TOKEN@github.com/pujianguo/drone_vue.git" gh-pages:gh-pages -f
    - echo https://pujianguo.github.io/drone_vue
  </pre>
  <h3>git version: {{gitVersion}}</h3>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      gitVersion: process.env.VUE_APP_GIT_VERSION,
    }
  },
}
</script>

<style>
#app {
  width: 800px;
  margin: 0 auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
h1{
  text-align: center;
}
pre{
  padding: 20px;
  border: 1px solid #2c3e50;
}
h3{
  text-align: right;
}
</style>
