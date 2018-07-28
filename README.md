# iOS-FullRoot-Access
Create components for iOS full root access. (afc2d)

## テスト環境
* macOS 10.13.5
* iOS 11.3.1

## 準備
* brewコマンドが使えるようにHomebrewを導入しておくこと (https://brew.sh/index_ja)
* cmakeとopensslが使えるようにしておくこと (`brew install cmake openssl`)
* permission関連のエラーが出たら、エラーメッセージ内の該当するディレクトリを作成(`sudo mkdir hoge`)して実行権限を与えてあげると解決する(例： `sudo chown -R $(whoami) /usr/local/Cellar`)

## 作業
0. 各ファイルに実行権限を与える(`chmod +x hoge`)
1. _InstallDmgCmd_ を実行(`./InstallDmgCmd`)してdmgコマンドが使えるようにコンパイルする
2. _DecryptDmg_ を実行してdmgファイルを復号化する
3. _CreateComp_ を実行してコンポーネントを生成する
4. 生成されたコンポーネントを正しい位置に配置する(後述)

## 生成されるコンポーネント
- _DecryptDmg_ --> rootfs.dmg をマウントして、/usr/libexec/内の _afcd_ を取り出し、 <strong>afc2d</strong> にリネーム
- _CreateComp_
  - afc2dService.dylib, afc2dService.plist --> /Library/MobileSubstrate/DynamicLibraries/ に配置
  - afc2d --> /usr/libexec/ に配置　(一応パーミッション確認 `chmod +x /usr/libexec/afc2d`)

## 仕上げ
iOSデバイス側で、`killall -9 lockdownd`　を実行して完了。PCからFull Root Accessが可能になる。
