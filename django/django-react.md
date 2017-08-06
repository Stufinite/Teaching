# Django + React 教學
## [出處](http://geezhawk.github.io/using-react-with-django-rest-framework)

### 環境：
* django：
  * start a new django project:`python manage.py startproject yourname`
  * 參考第1篇的django教學，可以讓你讀取到html即可 [第1篇django tutorial](http://slides.com/campass/deck#/)
  * 安裝react套件：`pip install django-webpack-loader`
  * settings.py修改這些部份，已經有的部份就是修改，沒有的部份就新增：
    ```
    INSTALLED_APPS = (
        ...
        'webpack_loader' # 新增這一行，上面的...不要貼近去
        ...
    )
    STATICFILES_DIRS = (
        #This lets Django's collectstatic store our bundles
        os.path.join(BASE_DIR, 'assets'),
    )
    WEBPACK_LOADER = {
        'DEFAULT': {
            'BUNDLE_DIR_NAME': 'bundles/',
            'STATS_FILE': os.path.join(BASE_DIR, 'webpack-stats.json'),
        }
    }
    ```

  * 然後在html裏面新增這個區塊：
    * 在html第1行貼這個：
      `{% load render_bundle from webpack_loader %}`這樣django才能把react資料載進來
    * 貼在body裏面，react會把html給放進這個div裏面，注意id跟react裏面`ReactDOM.render(... document.getElementById('container'))` 的id是不是一樣：
      ```
      <div id="container"></div>
      {% render_bundle 'main' 'js' 'INDEX' %}
      ```
    * 這邊我把 {% render_bundle 'main' %} 參照出處做修改，如果有遇到錯誤可以嘗試。

* npm:
  * 開發新的專案：
    * 初次建立專案：`npm init`（等同於python的`requirements.txt`，`npm install`就會去把`package.json`裏面的套件全部安裝）
    * 安裝必要的插件：`npm install --save-dev jquery react react-dom webpack webpack-bundle-tracker babel-loader babel-core babel-preset-es2015 babel-preset-react`

  * 注意 django 專案名稱不可和插件名稱重複
  * partner已經開發過，單純安裝環境：
    * 安裝npm需要的套件即可：`npm install`
* webpack:
  1. 在django的project目錄下(`manage.py那層`)：執行`mkdir -p assets/js`(mkdir是新增資料夾)，然後執行：`touch assets/js/index.js`(touch意思是新增一個index.js再assets/js這個路徑下)
  2. 把這份檔案貼進去assets/js/index.js裏面：[react範例](./index.js)
  3. 然後在project根目錄執行：`touch webpack.config.js`（touch意思是新增一個叫作webpack.config.js的檔案）
    * 貼入：[webpack.config.js的內容](./webpack.config.js)
    * 執行：編譯react語法轉換成js：`./node_modules/.bin/webpack --config webpack.config.js` 每次修改完js檔，都需要執行一次這個指令！！！
    * 說明：`webpack.config.js` 的作用，是告訴webpack這個套件，他應該要去compile哪一份js檔，把裏面reactjs的語法，轉換成瀏覽器能夠執行的js語法。（注意：webpack.config.js這個檔名不需要固定，可以自己取）
      * 如果有多個頁面需要經過webpack轉換，假設有兩頁（一頁叫作index.js,另一頁叫作inside.js）：
        * 可以把webpack.config.js重新命名，改成webpack.config.index.js以及webpack.config.inside.js，把entry以及ouput改掉

        ```
        entry: ['./assets/js/頁面js檔的路徑'],

        output: {
            //where you want your compiled bundle to be stored
            path: path.resolve('./assets/bundles/希望儲存頁面js檔的路徑'),
        ```  

        然後分別執行 `./node_modules/.bin/webpack --config webpack.config.index.js` 以及 `./node_modules/.bin/webpack --config webpack.config.inside.js` 就能夠用個別的config去編譯不同頁面的js檔了。
  4. 編譯完之後，執行`python manage.py runserver`在127.0.0.1:8000的django頁面就會看到Reactjs的結果了
