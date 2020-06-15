# Pipenv

###### tags: `python`, `pipenv`
[toc]

## 如何用 pipenv 初始一個專案 ?

```
# ~/PythonProjects/my_project_with_pipenv
$ pipenv --python 3.5        # init a virtualenv with python3.5
```
要初始一個全新的專案最標準的作法是用 pipenv --python [python版本號] 如此就可以指定一個 python 版本初始一的該專案（資料夾）的 “ virtualenv” 。這裡要注意，所指定的 python 版本一定要是你電腦上有的版本喔，如果使用電腦上沒有的版本是會出錯的喔！那如果你當下使用的 python 版本並不是大家所要使用的版本或是想未來想要更改，也不用擔心，等待初始完產生 Pipfile 檔後，就可以直接進入檔案手動更改任意版本囉。
用 pipenv 初始一個 virtualenv 基本上會發生兩件事情：
自動產生最核心的檔案 Pipfile 檔
在電腦預設路徑產生對應的 virtualenv 資料夾

## 最核心的檔案 Pipfile 簡單說明
Pipfile 自動生成後初始的檔案為：
```
# ~/PythonProjects/my_project_with_pipenv/Pipfile
[[source]]
name = "pypi"
url = "https://pypi.org/simple"
verify_ssl = true
[dev-packages]
[packages]
[requires]
python_version = "3.5"
```
`[[source]]` 是比較少人會更動到的，基本上可以保留不要動。`url` 此處宣告你之後要透過 pipenv 「去哪裡」下載套件們，default 是官方網站 PYPI。 `verify_ssl` 是可以調整是否要使用 SSL 安全防護機制。
`[dev-packages]` 和 `[packages]` 是未來你透過 pipenv install 安裝套件時會把安裝的套件名稱與版本紀錄在此，管控這個專案的套件版本。其中[dev-packages] 和 [packages] 顧名思義取決於在開發的環境到底是 production 還是 development（一般來說 development 開發環境會使用到比較多的套件輔助測試、debug 或團隊協同，而 production 開發環境則會使用最後產品要上線所需之套件）。詳細的安裝操作與步驟在下一小節。

`[requires]` 是用來管控此專案的 python 版本。比大小的 prefix 不只一種，有： * 、>、 >=、 <、 <= 、== (和不加 prefix 相同)。例如：
* python_version = ">=3.6"
* python_version = "==3.5.2"
* python_version = "3.2.5"
* python_version = "*" (允許全部版本)

注意「比較版本的大小」如果限制式只有到第一位版本號那麼第二位以下就不管，如果限制式只有到第二位版本號，那麼第三位就不管。例如：
* "python>=3.5" 那麼所有 3.5.x 以上的都可以使用
* "python<=3" 那麼所有 2.x.x 和 3.x.x 都可以使用（以2018年來說，就是全部版本xD）

此外，有一點必須注意，此處的 [[requires]] 乍看單字意思好像也可以套用在套件版本限制上，但這是錯的！目前這個地方有用的只是控制 python 版本而已，要限制 python 套件版本是寫在前面 [[packages]] 和 [[dev-packages]] 。

## 用 pipenv 安裝套件做套件版本控制

### pipenv install
用 pipenv 安裝套件最重要的一點就是「要在與 Pipfile 檔案同一層的路徑下指 令」！！！這是因為 pipenv 需要讀取當下路徑去尋找對應的 virtualenv 資料夾位置。
以下範例給出「默認版本安裝」、「指定版本安裝」與「development 安裝」三個 commands 以及「卸載」的 command：

```
# ~/PythonProjects/my_project_with_pipenv
$ pipenv install numpy  # install default version
$ pipenv install requests==2.18.1  # specify the wanted version
$ pipenv install requests==2.18.0 -d  # for development environment
$ pipenv uninstall numpy
```
而進行pipenv install 時，pipenv 至少做了四件事情：
* 讀取當下路徑與資料夾名稱，做 hash 編碼去尋找電腦中編碼相同的 virtualenv 資料夾位置。
* 安裝指定套件到剛剛找到的 virtualenv 資料夾中。
* 在 Pipfile 檔中的 [[packages]] 添加該套件名稱與版本。
* 在 Pipfile.lock 中添加安裝套件與其 dependencies 。

其中關於 pipenv 在安裝機制上最重要的就是這兩個文件：
* Pipfile ：負責主要安裝套件的版本控制。
* Pipfile.lock ：負責主套件之 dependencies (依賴套件)的版本控制，並控制下載套件的 source。

### Pipfile
安裝後 Pipfile 就會被更新：
```shell=
# ~/PythonProjects/my_project_with_pipenv/Pipfile
[[source]]
name = "pypi"
url = "https://pypi.org/simple"
verify_ssl = true
[dev-packages]
requests = "==2.18.0"
[packages]
numpy = "*"
requests = "==2.18.1"
[requires]
python_version = "3.5"
```
其中值得注意的是，安裝默認版本的 numpy 套件在 Pipfile 檔中並沒有寫上當下安裝的版本，而是寫上 * 允許所有版本，如果到時候利用 Pipfile 安裝則會使用 stable version。

### Pipfile.lock
除了 Pipfile 檔會被更新外， Pipfile.lock 檔也會被更新。 Pipfile.lock 檔是用來管理 dependencies 的。以下是安裝 requests 後更新完的 Pipfile.lock 檔：
```shell=
# ~/PythonProjects/my_project_with_pipenv/Pipfile.lock
{
    "_meta": {
        "hash": {
            "sha256": "50b8935d42537b1d392d8fe9a921302d58566b8b364c66af879714c0220b4483"
        },
        "pipfile-spec": 6,
        "requires": {
            "python_version": "3.5"
        },
        "sources": [
            {
                "name": "pypi",
                "url": "https://pypi.org/simple",
                "verify_ssl": true
            }
        ]
    },
    "default": {
        "certifi": {
            "hashes": [
                "sha256:47f9c83ef4c0c621eaef743f133f09fa8a74a9b75f037e8624f83bd1b6626cb7",
                "sha256:993f830721089fef441cdfeb4b2c8c9df86f0c63239f06bd025a76a7daddb033"
            ],
            "version": "==2018.11.29"
        },
        "chardet": {
            "hashes": [
                "sha256:84ab92ed1c4d4f16916e05906b6b75a6c0fb5db821cc65e70cbd64a3e2a5eaae",
                "sha256:fc323ffcaeaed0e0a02bf4d117757b98aed530d9ed4531e3e15460124c106691"
            ],
            "version": "==3.0.4"
        },
        "idna": {
            "hashes": [
                "sha256:156a6814fb5ac1fc6850fb002e0852d56c0c8d2531923a51032d1b70760e186e",
                "sha256:684a38a6f903c1d71d6d5fac066b58d7768af4de2b832e426ec79c30daa94a16"
            ],
            "version": "==2.7"
        },
        "requests": {
            "hashes": [
                "sha256:65b3a120e4329e33c9889db89c80976c5272f56ea92d3e74da8a463992e3ff54",
                "sha256:ea881206e59f41dbd0bd445437d792e43906703fff75ca8ff43ccdb11f33f263"
            ],
            "index": "pypi",
            "version": "==2.20.1"
        },
        "urllib3": {
            "hashes": [
                "sha256:61bf29cada3fc2fbefad4fdf059ea4bd1b4a86d2b6d15e1c7c0b582b9752fe39",
                "sha256:de9529817c93f27c8ccbfead6985011db27bd0ddfcdb2d86f3f663385c6a9c22"
            ],
            "version": "==1.24.1" 
        }
    },
    "develop": {}
}
```
Pipfile.lock 檔為一個 nested json-formatted file ，可以從範例中看出我們明明只有安裝 requests 卻仍有安裝其他五個 requests 所依賴的套件，這部份只會出現在 Pipfile.lock 檔中並不會在 Pipfile 裡出現。

另外值得一提的是，可以在 Pipfile.lock 檔中看到每個套件都有 hashes attribute ，此處是用 source 來生成 hash 值，可以確保未來在安裝同樣套件名稱時，來源是一致的，不會下載到具有相同套件名稱卻來自其他來源的套件，也可以避免使用到其他惡意來源，是非常謹慎的一個設計。

實務上，上傳 Python projects 到 github 時一定會附上 Pipfile 以及 Pipfile.lock 檔喔！不然會被夥伴殺死的喔！哈哈哈 xD

### pipenv install 的小陷阱 
如果在使用 pipenv install 時不小心安裝錯誤，例如打錯套件名稱或是套件名稱不存在等（以下範例是我故意打錯套件名稱 pipenv install requestsss）：

此時通常你會再把名稱改對之後再下載一次對吧？但你會發現，儘管這次套件名稱打對了，依舊會噴一模一樣的 error message！！！

這是因為即使上次的套件名稱因為打錯名稱無法下載，pipenv 仍然把上一次書入錯誤的套件名稱加入 Pipfile 檔，又因為這次下載新套件前都會把 Pipflie 裡面的未安裝的套件再安裝好才會安裝新套件，此時 pipenv 就會被迫再一次的下載名稱錯誤的套件……

解法就是：直接去改寫 Pipfile 檔，把該錯誤的套件名稱拿掉！！！或是使用 pipenv uninstall 錯誤套件名稱 卸載套件同時也會自動移除 Pipfile 檔中的錯誤套件名！！！

## pipenv 的 virtualenv 到底是什麼 ?!
在初始化一個專案的 virtualenv 時，會以路徑與資料夾名 hash 出來一個 8 碼的編碼，這就是這個 virtualenv 的 name，並以此 name 新創一個資料夾放在 ~/.local/share/virtualvenvs/ 裡。以上面的範例為例，我的電腦會創一個 ~/.local/share/virtualvenvs/my_project_with_pipenv-7zI82k0Q/ 的資料夾。此資料夾裡面有：
```shell=
# ~/.local/share/virtualvenvs/my_project_with_pipenv-7zI82k0Q/
$ ls
bin/    include/    src/    lib/
```
有沒有發現這個資料夾就是一個小型的 python 獨立環境呀！裡面有套件儲存的地方（在 lib/ 裡面），也有 python 的 bin 檔（在 bin/ 裡面）！

每個系統會有不一樣的路徑，所以想要知道自己電腦上 virtualenv 路徑在哪裡，請按壓以下 command 就可以知道：

```shell=
# ~/PythonProjects/my_project_with_pipenv/
$ pipenv --venv
/home/.local/share/virtualvenvs/my_project_with_pipenv-7zI82k0Q/
```
沒錯！所謂的 pipenv 的 virtualenv 不過就是把每一個專案所需的一切裝在一個又一個的獨立資料夾中，當要跑某專案時，就來對應的 virtualenv 資料夾中找尋執行檔或套件！！當然實際上還有一些細節，像是 virtualenv 的環境變數等等，不過大致上這樣簡化解釋也不為過！

那你可以會想說：「不對啊，那如果好多個專案明明共用某些套件，那這樣不就每個 virtualnev 資料夾都要裝一模一樣的套件，不是很浪費空間嗎？！」

這樣說沒錯，但也不太對。主要是我們實務上在做專案時，如果某個專案一陣子不會操作或是告一個段落甚至是大功告成，我們就會把 virtualenv 資料夾刪除，等到時候要再用的時候在安裝一次就可以！要移除 virtualenv 資料夾的 command 是：
```shell=
# ~/PythonProjects/my_project_with_pipenv/
$ pipenv --rm
```

## 在 pipenv virtualenv 裡執行程式碼

### pipenv run
```shell=
# ~/PythonProjects/my_project_with_pipenv/
$ pipenv run python3 test.py
```
例子中，要在 virtualenv 中執行 test.py 檔，必須在前面加上 pipenv run 。在 pipenv run 後面加的所有 command 都會在 virtualenv 裡頭去執行，所以例子中 pipenv run 後面去執行 python 程式時，會去找 virtualenv 資料夾中的套件，而不是 local (本機)上原先有的套件喔！！

所以，如果想使用 virtualenv 環境中的套件等，就一定要在想執行的 command 前面加 pipenv run 這個 command 喔！

如果下：
```shell=
$ python3 test.py
```
它會變成原先的模式，用 local 本機上安裝的套件去執行程式碼，就沒有達到我們要做的套件版本控制了。

### pipenv shell
如果你覺得每次下指令都要在前面加 pipenv run 很麻煩，pipenv 還有提供一個更方便的方式 pipenv shell：
```shell=
# ~/PythonProjects/my_project_with_pipenv/
$ pipenv shell
(my_project_with_pipenv-7zI82k0Q)
$ python3 test.py
```
當你先下了 pipenv shell 之後，terminal 就會進入 virtualenv shell 模式，之後下的每個指令都當作是在 virtualenv 中下一樣，省去了每次都要打 pipenv run 的麻煩。

如果要退出 virtualenv shell 模式，則要：
```shell=
# ~/PythonProjects/my_project_with_pipenv/
(my_project_with_pipenv-7zI82k0Q)
$ exit
```

## pipenv 指令筆記與其他功能
pipenv 的功能太多，在加上各種 args 排列組合，不勝枚舉，在此將文中所有提過的指令羅列出來，並列舉一些筆者常用的 commands 吧！

* pipenv --python 版本
* pipenv install 套件 [-d]
* pipenv uninstall 套件
* pipenv --rm
* pipenv --venv
* pipenv run 指令
* pipenv shell
* pipenv lock ：當沒有Pipfile.lock 時，根據當下 virtualenv 資料夾中所有套件的版本，生成新的 Pipfile.lock 檔。
* pipenv sync ：安裝所有 Pipfile.lock 中指定的版本套件。
* pipenv graph ：列出套件安裝的 dependency tree (把 Pipfile.lock 以 dependency tree 方式倒出)。


:::info
:bulb: **Reference:** 
* https://medium.com/citycoddee/python-%E5%B7%A5%E5%85%B7%E7%AE%B1-1-%E5%B0%88%E6%A1%88%E5%BF%85%E5%82%99-pipenv-a517e292f6c
:::

