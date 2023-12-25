---
title: ChatdollKitで作った対話アバターをwebブラウザ上で動作させる方法
tags:
  - WebGL
  - Web
  - Unity
  - ChatdollKit
private: false
updated_at: '2023-10-06T02:00:16+09:00'
id: f06cab1e936e6cdecebf
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに

ChatdollKitで作ったアバターと話せるシステムをwebGLでビルドする

## ChatdollKit

ChatdollKitは、会話できるアバターを作成するためのフレームワークです
機能やオプションがたくさんあるので、Unityで話せるアバターを作りたい人におすすめです

※ChatdollKitを使った対話アバターの作り方は[こちら](https://qiita.com/ayakakawabe/items/9649745586323edb83e2)

## webGL
webGLとは、Unityで作ったプロジェクトをwebブラウザ上で動作させるための仕様のことです

ChatdollKitもwebGLに対応しているのですが、そのままwebGLでビルドしようとしたらエラーが出たので、この記事ではその解決方法を紹介します

今回の記事は[ChatdollKitのREADME](https://github.com/uezo/ChatdollKit/blob/master/README.ja.md#-webgl%E3%81%A7%E3%81%AE%E5%AE%9F%E8%A1%8C)を参考にさせていただきました

# 開発環境

* Windows10
* Unity - 2021.3.6f1
* chatdollKit - v0.6.1
* uLipSyncWebGL - v0.2

※対話アバターの開発環境は[こちら](https://qiita.com/ayakakawabe/items/9649745586323edb83e2#%E9%96%8B%E7%99%BA%E7%92%B0%E5%A2%83)

# エラーの原因

webGLでビルドしようとしたら、コンソールにエラーが表示されました
```
The name 'Microphone' does not exist in the current context
```
MicrophoneはuLipSyncで使用しているのですが、webGLでは使えないみたいです

# 解決方法
Microphoneの代わりにChatdollMicrophoneを使います

1. [uLipSyncWebGLのgithub](https://github.com/uezo/uLipSyncWebGL/releases)から、uLipSyncWebGLEdition_2.2.0.unitypackageをダウンロードします
1. ダウンロードしたuLipSyncWebGLEdition_2.2.0.unitypackageを開いてインポートします
1. Assets > uLipSyncWebGL > Scripts のuLipSyncWebGL.unitypackageをChatdollKitVRMのInspectorにドラッグ＆ドロップで追加します
![uLipSyncWebGL.unitypackage.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2784580/3d2b76d6-653a-70bc-5fe7-3e2fcdf89892.jpeg)
![uLipSyncWebGL.unitypackage_attached.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2784580/103d8105-89cf-40f2-703b-c767d57fdcb7.jpeg)
1. uLipSyncにある下記の3つのファイルを消去します
    * uLipSync > Runtime > uLipSyncMicrophone
    * uLipSync > Runtime > Core > MicUtil
    * uLipSync > Editor > uLipSyncMicrophoneEditor
        * 公式READMEにはuLipSyncMicrophoneEditorと書いてありますが、uLipSync (v2.6.1)ではuLipSyncMicroputEditorというファイル名でした
        * もしuLipSyncMicrophoneEditorがなければ、uLipSyncMicrophoneとMicUtilを消去して実行してみて、エラーに書いてあるファイルを消去すればいいと思います
1. uLipSync > Editor > EditorUtilのファイルを開き、DrawMicSelectorメソッドをコメントアウトする
ファイルはダブルクリックで開けます
![EditorUtil.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2784580/d27d0c6e-3d1d-a54b-5791-fa5725f36996.jpeg)

# webGLでのビルド方法
ビルド方法は下記の記事が分かりやすいと思います
[UnityでWebGLビルドする際の注意点](https://alterbo.jp/blog/hisshi4-2011/)
[Unity：作ったゲームをWebGLビルドで公開する](https://dianxnao.com/unity%EF%BC%9A%E4%BD%9C%E3%81%A3%E3%81%9F%E3%82%B2%E3%83%BC%E3%83%A0%E3%82%92webgl%E3%83%93%E3%83%AB%E3%83%89%E3%81%A7%E5%85%AC%E9%96%8B%E3%81%99%E3%82%8B/)

## 設定
1. UnityHubでwebGLでビルドするためのツールをインストールします
1. webGLでビルドしたいプロジェクトをUnityで開いている場合は、一度閉じて開き直します
1. File > Build Settings > webGL を選択し、SwitchPlatformをクリックします
1. Player Settings...をクリック
1. エラーが表示されている場合は、エラー内容に従って設定を変更します

### ビルドしたものをローカル環境で動かしたい場合
詳しくは[こちら](https://qiita.com/ayakakawabe/items/f66346437a954d2b032b)を参考にしてください

webGLでビルドすると、ビルド完了時は自動的にwebブラウザにUnityで作ったものが表示されますが、次回webブラウザ上で表示できなくなります
それを回避するためにPlayer Settingsで設定を変更します

* Enable ExceptionsをNonenに変更
* Compression FormatをDisabledに変更
![Player Settings.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2784580/b49d7c46-3a02-4809-fb27-f06c396ebf9e.jpeg)

## 実行

1. Build Settingsに戻り、Build And Runをクリックします
1. ビルドしたものを保存するフォルダを選択すると、ビルドが開始されます
webGLのビルドにはかなり時間がかかります
PCのスペックにもよりますが、10分以上はかかると思います
1. ビルドが終わると自動的にwebブラウザが開きます
![webGL_build.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2784580/95c8e5b5-5653-835b-a376-8fdd3be02ea9.png)

# ローカル環境で起動する方法
詳しくは[こちら](https://qiita.com/ayakakawabe/items/f66346437a954d2b032b)を参考にしてください

1. ターミナルかコマンドプロンプトを開き、ビルドしたファイルが保存されているフォルダに移動します
1. `$ python3 -m http.server 8000`と入力し、エンターを押します
1. http://localhost:8000/ にアクセスするとUnityで作ったプロジェクトを実行できます

# まとめ

今回はCHatdollKitをで作成したものをwebGLでビルドし、ローカル環境で動作させる方法について説明しました

## 参考資料
* [ChatdollKit GitHub（webGLでの実行）](https://github.com/uezo/ChatdollKit/blob/master/README.ja.md#-webgl%E3%81%A7%E3%81%AE%E5%AE%9F%E8%A1%8C)
* [uLipSyncWebGL GitHub](https://github.com/uezo/uLipSyncWebGL)
* [ChatdollKitとChatGPTで対話アバターを作成する](https://qiita.com/ayakakawabe/items/9649745586323edb83e2)
* [UnityでWebGLビルドする際の注意点](https://alterbo.jp/blog/hisshi4-2011/)
* [Unity：作ったゲームをWebGLビルドで公開する](https://dianxnao.com/unity%EF%BC%9A%E4%BD%9C%E3%81%A3%E3%81%9F%E3%82%B2%E3%83%BC%E3%83%A0%E3%82%92webgl%E3%83%93%E3%83%AB%E3%83%89%E3%81%A7%E5%85%AC%E9%96%8B%E3%81%99%E3%82%8B/)
* [webGLでビルドしたものをローカル環境で動作させる方法](https://qiita.com/ayakakawabe/items/f66346437a954d2b032b)







