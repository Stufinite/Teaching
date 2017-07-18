# Django + React 教學
## [出處](http://geezhawk.github.io/using-react-with-django-rest-framework)

### 環境：

* npm:
  * 開發新的專案：
    * 初次建立專案：`npm init`（等同於python的`requirements.txt`，`npm install`就會去把`package.json`裏面的套件全部安裝）
    * 安裝必要的插件：`npm install --save-dev jquery react react-dom webpack webpack-bundle-tracker babel-loader babel-core babel-preset-es2015 babel-preset-react`
  * partner已經開發過，單純安裝環境：
    * 安裝npm需要的套件即可：`npm install`
* webpack:
  * 在django的project目錄下(`manage.py那層`)：執行`mkdir -p assets/js`
  * 然後執行：`touch assets/js/index.js`
