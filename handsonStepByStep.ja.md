**Contents**

- Azure Blockchainの実習ラボステップバイステップ
    - このハンズオンについて
- 概要
    - ソリューションアーキテクチャ
    - ハンズオン用の環境
    - 課題1：Azure Active Directoryテナントをセットアップする
        - タスク1：Azure ADテナントを作成する
        - タスク2：アプリ登録を作成する
        - タスク3：Azure AD管理者グループを作成する
        - タスク4：管理者ユーザーを追加する
    - 課題2：Azure Blockchain Workbenchを展開する
        - タスク1：SSH公開鍵/秘密鍵の生成
        - タスク2：Azure Blockchain Workbenchをデプロイする
        - タスク3：Azure Blockchain Workbench WebクライアントのURLを取得する
        - タスク4：Reply URLを設定する
    - 課題3：Blockchain Workbench Webクライアントのデプロイの確認
        - タスク1：Blockchain Workbench Webクライアントを開く
    - 課題4：スマートコントラクトを作成する
        - タスク1：コードテレメトリコンプライアンスのスマートコントラクト
        - タスク2：設定ファイルの作成
        - タスク3：スマートコントラクトを展開する
    - 課題5：コントラクトペルソナへのユーザーの割り当て
        - タスク1：Azure AD でユーザーを作成する
        - タスク2：ユーザ割り当ての作成
    - 課題6：スマートコントラクトのインスタンスを作成して処理する
        - タスク1：コントラクトインスタンスの作成
        - タスク2：Contoso Shippingへの責任の移管
        - タスク3：シミュレートされたデバイスのテレメトリを取り込む
        - タスク4：ブロックチェーン配送の責任を引き受ける
        - タスク5：Northwindトレーダーへの最終配信
        - タスク6：コンプライアンスのためのスマートコントラクトの監査
    - ハンズオン後
        - タスク1：リソースの削除
        - タスク2：Azure ADテナントを削除する


# Azure Blockchainの実習ラボステップバイステップ

## このハンズオンについて
このラボでは、Azure Blockchain を使用して IoT 監査ソリューションの構築と設定をする方法を学習します。 これを行うには、商品の輸送中の状況に関連する契約(コントラクト)の詳細を強制するだけでなく、デバイスのテレメトリ情報を収集するために Ethereum のスマートコントラクトを使用します。 具体的には、IoTデバイスは温度と湿度のデータをレポートし、データはあらかじめ合意された許容可能なレンジに治るか、スマートコントラクトを通じて検証されることになります。

この実習ラボの最後では、Azure Blockchain Workbenchを展開および構成し、Solidityを使用してEthereum スマートコントラクトを作成および展開し、IoTとブロックチェーンの両方を単一のソリューションに統合するソリューションを構築することができます。

### 概要

このラボでは、Azure Blockchain Workbench を使用してブロックチェーンソリューションをデプロイします。 その後、Azure AD を使用して複数のユーザーの認証と承認を構成します。 次に、サプライチェーン内のパッケージのコンプライアンスを追跡および実施するためのスマートコントラクトが、Solidityで書かれたブロックチェーン内に実装されます。 最後に、ブロックチェーン上にスマートコントラクトのインスタンスを作成し、スマートコントラクトで定義されているビジネスワークフローを実行します。 また、シミュレートされたデバイスアカウントを使用してコントラクトが正しくワークしているかを検証するために、IoTデバイスセンサーテレメトリをスマートコントラクトに入力します。

## ソリューションアーキテクチャ

![Diagram of the solution architecture showing the major components and Azure services used to build the solution.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image2.png "Diagram of the solution architecture showing the major components and Azure services used to build the solution")

## ハンズオン用の環境

1. An Azure サブスクリプション

## 課題1：Azure Active Directoryテナントをセットアップする

この課題では、Azure Blockchain Workbench がユーザーの認証に使用する新しい Azure AD テナントを構成します。

Blockchain Workbenchでのユーザーの認証と承認は、Azure Active Directory（AAD）テナントを使用して実行されます。 競合の可能性を減らすために、このラボで使用するための新しいAADテナントを作成しましょう。

### 課題1：Azure Active Directoryテナントをセットアップする

1. 作成されたリソースを展開するために使用する Azure Subscription のユーザー/パスワードで Azureポータル<http://portal.azure.com> にログインします。
2. 左側のナビゲーションで、**+リソースの作成**、 **ID**、**Azure Active Directory** の順にクリックします。
3.  **Create directory** gで、次の値を入力して **Create** をクリックします。

    a.  Organization name: **Northwind Traders**

    b.  Initial domain name: **enter a unique domain name**.

    c.  Country or region: **Select your country / region**.

    ![The information above is entered and highlighted on the Create directory ブレード.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image7.png "Configure the Directory to create")


4. 新しいディレクトリが作成されるのを待ちます。 これには少し時間がかかります。
5. **ディレクトリの作成** 内で、 **here** のリンクをクリックします。 これにより、新しく作成されたディレクトリに移動します。

    ![The message with the link to navigate to the newly create Directory is shown.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image9.png "The message with the link to navigate to the newly create Directory is shown")

6. **Azure Active Directory** 、**カスタムドメイン名** の順にクリックします。

    ![Custom domain names is highlighted for the Azure Active Directory.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image10.png "Custom domain names is highlighted for the Azure Active Directory")

7. **Azure Active Directoryテナント**の**ドメイン名**をコピーします。 これは後で使用されます。

    ![The Domain Name for the newly created Azure Active Directory Tenant is highlighted in the list of custom domains.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image11.png "The Domain Name for the newly created Azure Active Directory Tenant is highlighted in the list of custom domains")

### タスク2：アプリ登録を作成する

1. **Azure Active Directory**gで、**App registrations**をクリックしてください。

    ![App registrations link is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-app-registrations-list.png "App registrations link is highlighted")

2. **App registrations** ペインで、**+新規アプリケーション登録**をクリックして新しいアプリを追加します。
    ![The +New application registration button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-create-appregistration-button.png "The +New application registration button is highlighted")

3. **Create**gで、次の値を入力して**Create**をクリックします。

    a. Name: **Azure Blockchain Workbench Web Client**

    b. Application type: **Web app / API**

    c. Sign-on URL: **http://localhost**

     ![The above values are set.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-app-registration-create.png "Create a new App registration")

4. **Registerd App**gに、**アプリケーションID**をコピーします。 後で参照するために保存してください。

    ![The Application ID is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-registeredapp-appid.png "The Application ID is highlighted")

5. **Registered App**gで、**Settings**をクリックしてから、Settingsペイン内の**Keys**をクリックします。

    ![The Settings button is highlighted and labeled 1, and Keys is highlighted under Settings and labeled 2.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-registeredadd-settings-keys.png "Navigate to the Keys for the App registration")

6. **Keys**gで、以下の値を使用して新しい**Password**を作成してから、**Save** をクリックします。

    a. Description: **Client Secret**

    b. Expires: **Never expires**

    ![The Description field is highlighted and labeled 1, and the Expires field is highlighted and labeled 2, and the Save button is highlighted and labeled 3.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-registeredapp-keys-create.png "Create a new Password")

7. 新しく作成したパスワードの**Password Value**をコピーします。 ここでパスワードが表示されるのはこのときだけなので、gを閉じる前に必ずこれを行ってください。

    ![The value for the newly created Password is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-registeredapp-key-value.png "Copy the value for the Password")

8. **Registered app**gで、**Manifest**ボタンをクリックしてエディタを開き、アプリケーション登録用のマニフェストJSONを編集します。

    ![The Manifest button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-registeredapp-manifest-button.png "The Manifest buton is highlighted")

9. **Edit manifest**gで、**appRoles**オブジェクト配列JSONを見つけます。

    ![The appRoles section of the Manifest JSON is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-registredapp-manifest-approles-empty.png "The appRoles section of the Manifest JSON is highlighted")

10. **appRoles**定義を次のJSONに置き換えます。 これにより、登録アプリケーションはAzure ADの "Administrator"グループを使用してBlockchain Workbench管理者を指定するように構成されます。

    ```json
      "appRoles": [
         {
           "allowedMemberTypes": [
             "User",
             "Application"
           ],
           "displayName": "Administrator",
           "id": "<A unique GUID>",
           "isEnabled": true,
           "description": "Blockchain Workbench administrator role allows creation of applications, user to role assignments, etc.",
           "value": "Administrator"
         }
       ],
    ```

11. **Edit manifest** における**\<A unique GUID\>**プレースホルダを一意のGUID値に置き換えます。

一意のGUIDを生成するには、ローカルで、またはAzureポータルの**Azure Cloud Shell** 内で **PowerShell**を開き、次のコマンドを実行します。
```bash
New-GUID
```

![The results of the PowerShell command is shown.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-registeredapp-cloud-shell-new-guid.png "The results of the PowerShell command is shown")

環境がbash なら下のコマンドでもよいでしょう。
```bash
python2 -c 'import uuid; print str(uuid.uuid1())'
```

**Note**: または、Webブラウザを開き、<https://www.guidgen.com/>に移動してWebブラウザ内から一意のGUIDを生成します。

12. 一意のGUIDがマニフェストエディタ内に配置されると、最終的なJSONは次のようになります。

    ![The final JSON that was edited is shown.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-registeredapp-manifest-approles-unqiue-guid.png "The final JSON that was edited is shown")

13. **Save**をクリックします。

    ![The Save button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-registeredapp-manifest-save-buton.png "The Save button is highlighted")

14. **Registered app**gで、**Setting**、**Required permission**の順にクリックします。

    ![Settings is highlighted and labeled 1, and Required permissions is highlighted and labeled 2 on the Settings ペイン.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-registeredapp-settings-required-permissions-link.png "Navigate to the Required permissions")

15. **Required permissions** ペインで, **+ Add**をクリックします。

    ![The Add button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-registeredapp-required-permissions-add-button.png "The Add button is highlighted")

16. **Add API access**gで、**Select an API**をクリックし、次に**Microsoft Graph**をクリックしてから、**Select**をクリックします。

    ![Select an API is highlighted and labeled 1, and Microsoft Graph is highlighted and labeled 2, and the Select button is highlighted and labeled 3.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-registeredapp-permission-step1-microsoft-graph.png "Select the Microsoft Graph API")

17. **Select permissions** ステップの **Enable Access**ペイン内で、スクロールダウンして**Read all users' full profiles*** パーミッションを選択し、次に **Select** をクリックします。

    ![Select permissions is labeled 1, and the Read all users' full profile permission is labeled 2, and the Select button is labeled 3.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-registeredapp-permission-step2-enable-access.png "Add the Read all users' full profile permission")

18.  **Done**をクリックして必要な権限を追加します。

19. **Required permissions** ペインで、 **Grant permissions** ボタンをクリックします、。

    ![Grant permissions button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-registeredapp-permission-grant-permission-button.png "Grant permissions button is highlighted")

20. 現在のディレクトリ内のすべてのアカウントに対する権限の付与を確認するには、**Yes**をクリックします。

    ![The Yes button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-registeredapp-permissions-confirm-grant.png "The Yes button is highlighted")

### タスク3：Azure AD管理者グループを作成する

1. **Azure Active Directory** で、**Groups**をクリックします。

    ![Groups is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-groups-button.png "Groups is highlighted")

2. **+New group**をクリック

    ![The +New group button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-groups-new-group.png "The +New group button is highlighted")

3. **Group** で、以下の値を入力し、 **Create**をクリックします。

    a. Group type: **Security**

    b. Group name: **Administrator**

    c. Membership type: **Assigned**

    ![The values listed above are highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-group-new-administrator-group.png "Create a new Group")

### タスク4：管理者ユーザーを追加する

1.  Azure Active Directorygで、**ユーザー**をクリックします。

    ![The Users link is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-users-button.png "The Users link is highlighted")

2. **All users**ペインで、現在Azure PortalにログインしているAzure ADユーザーをクリックします。

    ![Your Azure AD User is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-all-users-list-me.png "Navigate to your Azure AD User")

3. **User**gの**Groups**をクリックしてください。

    ![The Groups link is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-user-groups-button.png "The Groups link is highlighted")

4. Azure ADユーザーをAdministratorグループに追加するには、**+ Add**ボタンをクリックします。


    ![The +Add button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-user-add-group-button.png "The +Add button is highlighted")

5. **グループの選択**ペインで、**Administrator**グループをクリックし、**選択**をクリックします。

    ![The Administrator Group is highlighted and labeled 1, and the Select button is highlighted and labeled 2.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-user-add-group-select-administrator.png "Add the Administrator group")

6. **Azure Active Directory** で **Enterprise applications** をクリックします。

    ![The Enterprise application link is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-enterprise-applications-link.png "The Enterprise application link is highlighted")

7. **Enterprise applications - All applications** ペインで **Azure Blockchain Workbench Web Client** をクリックします。

    ![The Azure Blockchain Workbench Web Client application is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-enterprise-applications-list.png "The Azure Blockchain Workbench Web Client application is highlighted")

8.  **Enterprise Application** ブレード の **Azure Blockchain Workbench Web Client**にいき、**Users and groups**をクリックします

    ![The Users and groups link is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-enterprise-applications-users-and-groups-link.png "The Users and groups link is highlighted")

9.  **Users and groups** ペインで、**+ Add user** をクリックします。

    ![The +Add user button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-enterprise-applications-usersandgroups-adduser-button.png "The +Add user button is highlighted")

10.  **Add Assignment** ブレードで、**Users** をクリックし、現在Azure Portalにログインしているユーザーを選択し、**選択**をクリックします。

    ![Users is highlighted and labeled 1, and the user is highlighted and labeled 2, and the Select button is highlighted and labeled 3.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-enterprise-applications-usersandgroups-adduser-adminuser.png "Add the user you're currently logged in as")

11. **Role**が**Administrator**に設定されていることを確認してから、**Assign**をクリックします。

    ![Select Role is highlighted and labeled 1, and the Assign button is highlighted and labeled 2.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-enterprise-applications-usersandgroups-adduser-role-verify-assign-button.png "Add the Assignment")

## 課題2：Azure Blockchain Workbenchを展開する

この課題では、Azure Blockchain Workbenchを展開してセットアップします。

### タスク1：SSH公開鍵/秘密鍵の生成
(Windowsな方)
1. **PuTTY Key Generator** を使います。:

    <https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html>

2. **生成**をクリックして新しい公開/秘密鍵ペアの生成を開始し、指示に従ってマウスを動かしてキーをランダムに生成します。

    ![The Generate button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image37.png "The Generate button is highlighted")

3. 生成された公開鍵をファイルに保存するには、**Save public key**をクリックします。

    ![The Save public key button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image38.png "The Save public key button is highlighted")

4. **Save public key as:** ダイアログで、公開鍵ファイルを保存するフォルダの場所を選択し、ファイルに**PublicKey.txt**という名前を付けて、**Save**をクリックします。

    ![The PublicKey.txt file to save and create is selected.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image39.png "The PublicKey.txt file to save and create is selected")

5. 生成された秘密鍵をファイルに保存するには、**Save private key**をクリックします。

    ![The Save private key button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image40.png "The Save private key button is highlighted")

6. "*Are you sure you want to save this key without a passphrase to protect it?"* と表示されたら**はい**をクリックします。 通常、秘密鍵の安全性を高めるためにパスフレーズを設定しますが、このラボではそれをスキップできます。

    ![The warning dialog is shown.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image41.png "The warning dialog is shown")

7.  **Save private key as:**ダイアログで、以前に使用したのと同じフォルダを選択し、秘密鍵ファイルに**PrivateKey.ppk**という名前を付けて、**保存**をクリックします。

    ![The PrivateKey.ppk file to save and create is selected.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image42.png "The PrivateKey.ppk file to save and create is selected")

8. これでPuTTY Key Generatorを閉じることができますが、Keyファイルが保存された場所を覚えておいてください。今後のステップで必要となります。


(Linux/Mac)
1. ターミナルで コマンドを入力します
```bash
$ ssh-keygen 
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/hogehoge/.ssh/id_rsa): /<ホームフォルダへのパス>/.ssh/blockchain
Enter passphrase (empty for no passphrase):  (何も入力しない)
Enter same passphrase again: (何も入力しない)
Your identification has been saved in /Users/hogehoge/.ssh/blockchain.
Your public key has been saved in /Users/yukihattori/.ssh/blockchain.pub.
The key fingerprint is:
SHA256:xIy5edHogeHogeFHbrGZ4sfgOWnTF1ZC7UW2TOXPStg foobar@Hogehoge
The key's randomart image is:
+---[RSA 2048]----+
|          .   .+=|
|       =   + .oo+|
|      o + +   .==|
|       + + . + =O|
|                .|
|       o o  o.OB.|
|                .|
|            . + .|
|                 |
+----[SHA256]-----+

```

すると、キーが作成されます

```bash
$ ls ~/.ssh/blockchain*
/Users/hogehoge/.ssh/blockchain	/Users/hogehoge/.ssh/blockchain.pub
```
### タスク2：Azure Blockchain Workbenchをデプロイする

1. 再びポータルにもどります

3. **+Create a resource**をクリックして新しいリソースのプロビジョニングを開始します。

    ![Create a resource link is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image44.png "Create a resource link is highlighted")

4. **Marketplace Search**ボックスに**Azure Blockchain Workbench**と入力し、**Enter**を押します。

    ![The search field is highlighted with Azure Blockchain Workbench entered.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image45.png "Search for Azure Blockchain Workbench")

5.  検索結果で、**Azure Blockchain Workbench**をクリックします。

    ![Azure Blockchain Workbench is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image46.png "Azure Blockchain Workbench is highlighted")

6.  **Create**をクリック

    ![The Create button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image47.png "The Create button is highlighted")

7.   **Create Azure Blockchain Workbench**ブレードの**Basic**ステップで、次の値を入力して、**OK**をクリックします。

    a.  Resource prefix: リソースの命名のベースとして使用するプレフィックスを選択してください。

    b.  VM user name: demouser

    c. Authentication type: SSH public key

    d.  SSH public key: 以前に作成したPublicKey.txt (Macの場合はblockchain.pub)キーファイルの内容を貼り付けます

    ![The contents of the PublicKey.txt file is displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image48.png "The contents of the PublicKey.txt file is displayed")

    e.  Database password: Demo@pass123

    f.  Deployment Region: Southeast Asia

    g.  Resource group: BlockchainLab

    h.  Location: Southeast Asia

![The values listed above are entered and highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image49.png "Create Azure Blockchain Workbench")

8. **Azure Active Directoryのセットアップ** ステップ、次の値を入力してから、** OK **をクリックします。(現在、Advanced Setting ステップに統合されています。)

    a.  Azure AD tenant Domain name: ブロックチェーンワークベンチに使用するAzure ADテナントのドメイン名を入力してください。

    b.  Azure AD client Application ID: 以前にコピーした Azure ADクライアントアプリケーションID を入力してください。

    c.  Azure AD client Application key: 以前にコピーした Azure ADクライアントアプリケーションキーを入力してください。(2018年12月時点でこの設定画面においては不要)

    ![The value listed above are entered and highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image50.png "Configure Azure AD for the Azure Blockchain Workbench to create")

9. **ネットワークサイズとパフォーマンス** ステップ、デフォルト値を残して **OK**をクリックします。(現在、Advanced Setting ステップに統合されています。)

    ![The OK button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image50b-networksizeandperfstep.png "The OK button is highlighted")




10. **Azure Monitor**ステップで、デフォルト値をそのままにして**OK**をクリックします。(現在、Advanced Setting ステップに統合されています。)

    ![The OK button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image50c-azuremonitor.png "The OK button is highlighted")

11. 検証が完了したら、**Summary**ステップで**OK**をクリックします。

     ![The OK button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image51.png "The OK button is highlighted")

12. **Buy**ステップで、**Create**をクリックします。

    ![The Create button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image52.png "The Create button is highlighted")

13. **Blockchain Workbench**がデプロイされるのを待ちます。 完了までに約60〜90分かかることがあります。 これが終わるのを待ちます
    
    ![Azure Blockchain Workbench is provisioning.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image53.png "Azure Blockchain Workbench is provisioning")

### タスク3：Azure Blockchain Workbench WebクライアントのURLを取得する

1.  Azure Portalの左側にあるナビゲーションで**Resource groups**をクリックし、リストの**BlockchainLab**リソースグループをクリックします。

    ![Resource groups is highlighted and labeled 1, and the BlockchainLab resource group is highlighted and labeled 2.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image54.png "Select the BlockchainLab resource group")

2.  **BlockchainLab** のリソースグループブレードで、**TYPE** 列見出しをクリックしてリソースグループ内のリソースのリストをソートします。

    ![The Type column is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image55.png "The Type column is highlighted")

3.  リストの一番上に**App Service**リソースが表示されたら、名前の末尾に "-api"が*ない方の* App Serviceリソースをクリックします。 リソースがTYPE順にソートされている場合は、クリックするApp Serviceが最初になります。

    ![The Web App resource is highlighted in the list of Azure resources.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image56.png "Select the Web App resorce")

4. **App Service** ブレードに、将来使用するためにWeb Appの**URL**をコピーします。 メモ帳を開き、今後の参照用に.txtファイルに保存することをお勧めします。
    
    ![The URL is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image57.png "The URL is highlighted")


### タスク4：Reply URLを設定する

1.  ポータルに戻ります。

2.  正しいサブスクリプションが選択されているか確認します。

    ![The Directory + subscriptions menu button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image58.png "The Directory + subscriptions menu button is highlighted")

3.  Azureポータルの左側にあるナビゲーションで、**Azure Active Directory**をクリックします。

    ![Azure Active Directory link from the left-side menu of the Azure Portal is shown.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image59.png "Select Azure Active Directory")

4.  **Azure Active Directory**ブレードで、**App registration**をクリックします。

    ![App registrations is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image60.png "App registrations is highlighted")

5.  **App registration** ペインで、**View all applications**ボタンをクリックしてすべてのApp registrationを表示します。

    ![The View all applications button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/azuread-app-registrations-view-all-button.png "The View all applications button is highlighted")

6. リスト内の**Azure Blockchain Workbench Web Client** をクリックします。

    ![Azure Blockchain Workbench Web Client app registration is displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image61.png "Azure Blockchain Workbench Web Client app registration is displayed")

7.   **Registered app** ブレード で **Reply URLs** をクリックします。

    ![The Settings button is highlighted and labeled 1, and the Reply URLs link under General is highlighted and labeled 2.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image62.png "Open the Reply URL configuration")

8.  新しい **Reply URL**を追加し、以前に**Azure Blockchain Workbench Web App**からコピーした**URL**を貼り付けてから、**保存**をクリックします。

    ![The Reply URL is highlighted, and the Save button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image64.png "Save the Reply URL")

## 課題3：Blockchain Workbench Webクライアントのデプロイの確認

この演習では、Azure Blockchain Workbench Webクライアントにアクセスして、デプロイが想定通りに機能していることを確認します。 また、Ethereum ネットワークのステータスを確認します。

### タスク1：Blockchain Workbench Webクライアントを開く

1. シークレットモードまたはプライベートブラウザモードで新しいブラウザウィンドウを開き、**Blockchain Workbench Web Client**のURLに移動します。

2. プロンプトが表示されたら、Azure AD内の**Administrator**グループに以前に追加した**Microsoftアカウント**を使用してサイトにログインします。

    ![Login with the Microsoft Account for the Administrator user.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image66.png "Login with the Microsoft Account for the Administrator user")

3. ログインすると、**Azure Blockchain Workbench Webクライアント**のダッシュボードビューが表示されます。

    ![Azure Blockchain Workbench Web Client web application is displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image68.png "Azure Blockchain Workbench Web Client web application is displayed")

## 課題4：スマートコントラクトを作成する

この課題では、Ethereum ブロックチェーンを対象とする新しい Smart Contract を Solidity で作成します。

### タスク1：コードテレメトリコンプライアンスのスマートコントラクト

1. **Visual Studio Code**を開き、**File**、続いて**Open Folder...**をクリックします。

    ![The File menu is highlighted and open, with the Open Folder menu item highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image80.png "Select the Open Folder menu item")

2. **Open Folder**ダイアログで、**C:/HOL**フォルダを選択します。 フォルダがまだ存在しない場合は、作成してください。 (Macの場合、**mkdir ~/HOL** で作成しましょう)

    ![The HOL folder is selected, and the Select Folder button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image81.png "Select the HOL folder on the C drive")

3. **Explorer**ペインを展開し、**HOL**フォルダの上に移動して、**New File**ボタンをクリックします。

    ![The Explorer icon in the left-side menu is highlighted and labeled 1, and the New File menu item for the HOL folder is highlighted and labeled 2.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image82.png "Create a new file within the HOL folder")

4. ファイル名として**TelemetryCompliance.sol**を入力します。

    ![The newly created TelemetryCompoliance.sol file is displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image83.png "New file has been created")

5. **TelemetryCompliance.sol**ファイルを開き、ファイルの先頭に次の行を追加します。 この最初の行は、Solidityバージョン0.4.20以降用に書かれていて、バージョン0.5.0までは機能を壊さないように書かれていることを示しています。 これは、Smart Contractが新しいコンパイラバージョンで異なる動作をしないようにするためです。

    ```sol
    pragma solidity ^0.4.20;
    ```

6. 次に、ブロックチェーンワークベンチ の基本コントラクト WorkbenchBase を追加します。

    スマートコントラクトを作成するとき、この基本コントラクトはコントラクトの種類と住所を取得し、それをイベントとして記録します。 型と住所はコンストラクタから基本コントラクトに渡され、ContractCreated関数はコンストラクタから呼び出されます。 状態が変化すると、コントラクトは基本コントラクトのContractUpdated関数を呼び出して、更新が行われたことを示し、呼び出されたコントラクトの関数を指定します。

    ```sol
    contract WorkbenchBase {
        event WorkbenchContractCreated(string applicationName, string workflowName, address originatingAddress);
        event WorkbenchContractUpdated(string applicationName, string workflowName, string action, address originatingAddress);
    
        string internal ApplicationName;
        string internal WorkflowName;
    
        function WorkbenchBase(string applicationName, string workflowName) internal {
            ApplicationName = applicationName;
            WorkflowName = workflowName;
        }
    
        function ContractCreated() internal {
            WorkbenchContractCreated(ApplicationName, WorkflowName, msg.sender);
        }
    
        function ContractUpdated(string action) internal {
            WorkbenchContractUpdated(ApplicationName, WorkflowName, action, msg.sender);
        }
    }
    ```

7. 次に、スマートコントラクトの始まりを追加します。 このコントラクトのタイプ名はTelemetryComplianceになり、WorkbenchBaseの基本コントラクトから継承されます。
    
    ```sol
    contract TelemetryCompliance is WorkbenchBase('TelemetryCompliance', 'TelemetryCompliance')
    {
        // insert smart contract code here
    }
    ```

8. 次に、スマートコントラクトの機能を記入する必要があります。 **TelemetryCompliance** クラス内に次の **enum StateType** 定義を追加することによってこれを開始します。

    ```sol
    enum StateType {
        Creating,
        Created,
        TransitionRequestPending,
        InTransit,
        FinalDelivery,
        Completed,
        OutOfCompliance
    }
    ```

9. **StateType**型の**State**プロパティを追加します。 これは、スマート契約の有効期間を通じて、連絡先の適切な状態を保存するために使用されます。

    ```sol
    StateType public State;
    ```

10. スマートコントラクトに次の追加のプロパティを追加します。 これらのプロパティは、ワークフローに参加するさまざまな個人を追跡するために使用されます。

    ```
    address public InitiatingCounterparty;
    address public Counterparty;
    address public PreviousCounterparty;
    address public RequestedCounterparty;
    address public Device;
    address public SupplyChainOwner;
    address public SupplyChainObserver;
    ```

- 追加された"**Counterparty**"プロパティは、パッケージを現在所有している、過去に所有していた、もしくは今所有しようとしているさまざまな組織内の個人を反映するために使用されます。
- スマートコントラクトによって評価される湿度と温度に関するテレメトリ情報を提供するための "**Device**"プロパティは、実際のデバイスとAzure IoT Hubの両方、またはデバイスをログインユーザーとしてシミュレートしている人の両方と連携できるようにするためのコントラクトです。
- "**SupplyChainOwner**"は、サプライチェーンを通じてパッケージを出荷している組織を表します。
- "**SupplyChainObserver**"は、サプライチェーンに参加していないが監視している可能性がある組織を表します。 オブザーバーの例としては、政府機関が挙げられます。

    > **Note**：SupplyChainObserverはスマートコントラクトのプロパティに含まれていますが、このラボでは実装されていません。

11. 次のプロパティを追加して、パッケージが温度と湿度の両方について最小と最大のしきい値（最小と最大）の読み取り値を持つ環境で維持されるようにします。 最後に報告された値と最後のセンサー更新のタイムスタンプをUnix epoch date valueとして記録するプロパティもあります。 

    ```sol
    int public MinHumidity;
    int public MaxHumidity;
    int public MinTemperature;
    int public MaxTemperature;
    int public LastHumidity;
    int public LastTemperature;
    uint public LastSensorUpdateTimestamp;
    ```

12. 次に、コントラクトのコンプライアンスステータスに関する情報を提供するためにいくつかのプロパティを追加します。 **ComplianceStatus**プロパティは、監視されているセンサー値がコンプライアンスから外れているかどうかを格納します。 **enum** の **SensorType**は**ComplianceSensorType** プロパティと共に使用され、記録された特定のセンサー測定値に関する情報を格納します。 **ComplianceSensorReading**プロパティはセンサーの読みの実際の値を格納します。 また、**ComplianceDetail**プロパティにはコンプライアンスに関する追加情報が格納されます。
    
    ```sol
    enum SensorType { None, Humidity, Temperature }
    bool public ComplianceStatus;
    string public ComplianceDetail;
    SensorType public ComplianceSensorType;
    int public ComplianceSensorReading;
    ```

13. スマートコントラクトの機能の追加を開始するには、次に**TelemetryCompliance**スマートコントラクトに次の**コンストラクタ**を追加します。

    ```sol
    function TelemetryCompliance(address device, address supplyChainOwner, address supplyChainObserver, int minHumidity, int maxHumidity, int minTemperature, int maxTemperature) public
    {
        ComplianceStatus = true;
        ComplianceSensorReading = -1;
        InitiatingCounterparty = msg.sender;
        Counterparty = InitiatingCounterparty;
        Device = device;
        SupplyChainOwner = supplyChainOwner;
        SupplyChainObserver = supplyChainObserver;
        MinHumidity = minHumidity;
        MaxHumidity = maxHumidity;
        MinTemperature = minTemperature;
        MaxTemperature = maxTemperature;
        State = StateType.Created;
        ContractCreated();
    }
    ```

    コンストラクタ関数はスマートコントラクトと同じ名前を持ち、コントラクトが作成されたときに呼び出されます。 コントラクトは、コントラクトを作成した個人が**相手方**であると仮定し、またこの個人を**InitiatingCounterparty**に割り当てます。

    最初に、コンストラクタは、コンプライアンスを満たすために**ComplianceStatus**を**true**に設定し、まだセンサーの読み取りが行われていない場合、**ComplianceSensorReading**を **-1**に設定します。 コントラクトは**State**が、いま**Created**されたように設定します。 

14. 次に、スマートコントラクトに**IngestTelemetry**関数を追加します。 この機能は、パッケージ上のセンサーからデバイスのテレメトリ情報を受け取ります。 この関数のパラメータはセンサーからデータを受け取るための**湿度**と**温度**であり、次の**timestamp**パラメーターはセンサー情報が収集されたときのUnix Epoch Time表す整数です 。

    IngestTelemetry関数は以下のアクションを実行します。

    a. テレメトリの送信者がコントラクトに割り当てられているデバイスであることを確認します。

    b. 契約がテレメトリ情報の受信に関心がある状態にあることを確認します。コントラクトステートが"Completed" か、"Out of Compliance" でない状態です。

    c. LastHumidity、LastTemperature、およびLastSensorUpdateTimestamp に関連するプロパティに値を割り当てます。 

    d. コントラクトが作成されたときにそれらが規定の範囲の外にあるかどうか識別するために温度と湿度の遠隔測定値をチェックします。テレメトリが最小値と最大値の範囲内にない場合は、ComplianceStatusプロパティをfalseに設定することで契約は「準拠外」として記録されます。また、どのテレメトリがコンプライアンスの問題を引き起こしたかを記録します。

    > **Note**：センサーデータは継続的に収集されますが、継続的にデータが契約に送信されることはありません。代わりに、ビジネスルールによってデータが許容範囲外であることが確認されたときに送信されます。ラボには反映されていませんが、情報がxごとに1回送信されるシナリオもあります。ここで、xは5分、30分、1時間などの時間間隔です。

    
    ```sol
    function IngestTelemetry(int humidity, int temperature, uint timestamp) public
    {
        if (Device != msg.sender || State == StateType.OutOfCompliance || State == StateType.Completed)
        {
            revert();
        }
        
        LastHumidity = humidity;
        LastTemperature = temperature;
        LastSensorUpdateTimestamp = timestamp;
        
        if (humidity > MaxHumidity || humidity < MinHumidity)
        {
            ComplianceSensorType = SensorType.Humidity;
            ComplianceSensorReading = humidity;
            ComplianceDetail = 'Humidity value out of range.';
            ComplianceStatus = false;
        }
        else if (temperature > MaxTemperature || temperature < MinTemperature)
        {
            ComplianceSensorType = SensorType.Temperature;
            ComplianceSensorReading = temperature;
            ComplianceDetail = 'Temperature value out of range.';
            ComplianceStatus = false;
        }

        if (ComplianceStatus == false)
        {
            State = StateType.OutOfCompliance;
            /*When updating state */
            ContractUpdated("IngestTelemetry");
        }
    }
    ```

15. スマートコントラクトに**RequestTransferResponsiblity**関数を追加します。 この機能は、アイテムに対する責任を取るために他の当事者に要求することを管理します。 この機能は、要求に関連したアドレスが現在の*相手方*であり、コントラクトが転送が可能な状態にあるかどうかの評価を実行します。
    
    適切なコンテキストが設定されていない場合、*"revert()"*メソッドを使用して呼び出し側にエラーのフラグを立てます。
    適切なコンテキストが設定されている場合は、*RequestedCounterparty*プロパティを*newCounterparty*パラメータの値に設定します。

    ```sol
    function RequestTransferResponsibility( address newCounterparty ) public
    {
        if (Counterparty != msg.sender || (State != StateType.Created && State != StateType.InTransit) || newCounterparty == Device || newCounterparty == SupplyChainObserver)
        {
            revert();
        }
        RequestedCounterparty = newCounterparty;
        State = StateType.TransitionRequestPending;
        /*When updating state */
        ContractUpdated("RequestTransferResponsibility");
    }
    ```

16. 次に、**AcceptTransferResponsibility**関数を追加します。 この機能は、要求された当事者が品目の責任の移転を承認するためのプロセスを管理します。 この関数は、要求に関連付けられたアドレスが現在の*RequestedCounterparty*であり、コントラクトが*TransitionRequestPending*の状態にあるかどうかを評価します。
    
    適切なコンテキストが設定されていない場合は、 *"revert()"*メソッドを使って呼び出し側にエラーを通知します。

    適切なコンテキストが設定されている場合は、*PreviousCounterparty*プロパティを現在の*CounterParty*プロパティの値に設定します。 次に、現在の*CounterParty*を、責任の移管を受け入れた*RequestedCounterparty*に設定します。 状態を "*InTransit*"に遷移させ、*RequestedCounterparty*を0x0にリセットします。

    ```sol
    function AcceptTransferResponsibility() public
    {
        if (RequestedCounterparty != msg.sender || State != StateType.TransitionRequestPending)
        {
            revert();
        }

        PreviousCounterparty = Counterparty;
        Counterparty = RequestedCounterparty;
        RequestedCounterparty = 0x0;
        State = StateType.InTransit;
        /*When updating state */
        ContractUpdated("AcceptTransferResponsibility");
    }
    ```

17. **TakeFinalDelivery**関数を追加してください。 この機能は、パッケージの最終納入がいつ行われたかを識別するために使用されます。 関数は、関数を終了する要求に関連したアドレスが現在の*相手方*であるかどうか、およびパッケージが *"InTransit"*の状態にあるかどうかを判別するための評価を実行します。
    
    そうでなければ、 *"revert()"*メソッドを使って呼び出し側にエラーを知らせます。
    
    もしそれが*相手方*であれば、それは**State**プロパティを通して**FinalDelivery**に契約の状態を設定します。

    ```sol
    function TakeFinalDelivery() public
    {
        if (Counterparty != msg.sender || State != StateType.InTransit)
        {
            revert();
        }

        State = StateType.FinalDelivery;
        /*When updating state */
        ContractUpdated("TakeFinalDelivery");
    }
    ```

18. **Complete** 関数を追加してください。 この機能により、**SupplyChainOwner**は、契約が最終納入状態になったときに契約が完了したことを承認することができます。 この関数は、呼び出し側が*SupplyChainOwner*であり、契約が*FinalDelivery*状態にあることを確認します。
    
    そうでなければ、 *"revert()"*メソッドを使って呼び出し側にエラーを知らせます。
    
    もしそうであれば、それは契約の*State*を*Completed*に設定し、*PreviousCounterparty*プロパティを*Counterparty*プロパティの現在の値に設定し、そして現在の*Counterparty*の値をリセットします。
    
    ```sol
    function Complete() public
    {
        if (SupplyChainOwner != msg.sender || State != StateType.FinalDelivery)
        {
            revert();
        }

        PreviousCounterparty = Counterparty;
        Counterparty = 0x0;
        State = StateType.Completed;
        /*When updating state */
        ContractUpdated("Complete");
    }
    ```

19. ファイルをセーブします。

    ![The File menu is selected and open, and the Save menu item is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image84.png "Save the file")

### タスク2：設定ファイルの作成

このタスクでは、スマートコントラクトのメタデータを含む設定ファイルを作成して、Blockchain Workbenchがコントラクト用のアプリケーションを生成できるようにします。

1.  **Visual Studio Code**で**C:/HOL**フォルダに開いたままで、**TelemetryCompliance.json**という名前の新しいファイルを作成します。

    ![The newly created TelemetryCompliance.json file is displayed and highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image85.png "Create a new file named TelemetryCompliance.json")

2.  **TelemetryCompliance.json**ファイルを開き、以下のJSONオブジェクトコードを追加します。

    ```json
    {
    "ApplicationName": "TelemetryCompliance",
    "DisplayName": "Telemetry Compliance",
    "Description": "",
    "ApplicationRoles": [
        {
            "Name": "Admin",
            "Description": "Admin"
        },
        {
            "Name": "User",
            "Description": "User"
        },
        {
            "Name": "Auditor",
            "Description": "Auditor"
        }
    ],
    "Workflows": [
        {
            "Name": "TelemetryCompliance",
            "DisplayName": "Telemetry Compliance",
            "Description": "",
            "Initiators": ["Admin","User"],
            "StartState": "Creating",
            "Properties": [
    
            ],
            "Constructor": {
                "Parameters": [
                ]
            },
            "Functions": [
    
            ],
            "States": [
    
            ]
        }
      ]
    }
    ```


3. "**Properties**" オブジェクト内に、次のJSONを追加してTelemtryComplianceスマートコントラクトのプロパティのメタデータを定義します。 これにより、スマート契約で定義されているプロパティのマッピングが作成されます。

    ユーザーやコントラクトなど、Solidityスマートコントラクトのアドレスに反映される項目が複数あるため、Blockchain Workbenchには、契約と適切にやり取りする方法を判断するのに役立つカスタムデータ型が用意されています。 *InitiatingCounterparty*、*Counterparty*、*PreviousCounterparty*、*RequestedCounterparty*、*Device*、*SupplyChainOwner*、および*SupplyChainObserver*プロパティはすべて"*user*"タイプとして定義されています。

    センサールールとコンプライアンスの詳細を追跡するための追加のプロパティもここで定義され、以前に作成されたスマートコントラクトの定義で見られたものと1：1のマッピングがあります。
    
    ```json
                {
                    "Name": "State",
                    "DisplayName": "State",
                    "Description": "Holds the state of the contract.",
                    "Type": {
                        "Name": "state"
                    }
                },
                {
                    "Name": "InitiatingCounterparty",
                    "DisplayName": "Initiating Counterparty",
                    "Description": "",
                    "Type": {
                        "Name": "user"
                    }
                },
                {
                    "Name": "Counterparty",
                    "DisplayName": "Counterparty",
                    "Description": "",
                    "Type": {
                        "Name": "user"
                    }
                },
                {
                    "Name": "PreviousCounterparty",
                    "DisplayName": "Previous Counterparty",
                    "Description": "",
                    "Type": {
                        "Name": "user"
                    }
                },
                {
                    "Name": "RequestedCounterparty",
                    "DisplayName": "Requested Counterparty",
                    "Description": "",
                    "Type": {
                        "Name": "user"
                    }
                },
                {
                    "Name": "Device",
                    "DisplayName": "Device",
                    "Description": "",
                    "Type": {
                        "Name": "user"
                    }
                },
                {
                    "Name": "SupplyChainOwner",
                    "DisplayName": "SupplyChain Owner",
                    "Description": "",
                    "Type": {
                        "Name": "user"
                    }
                },
                {
                    "Name": "SupplyChainObserver",
                    "DisplayName": "SupplyChain Observer",
                    "Description": "",
                    "Type": {
                        "Name": "user"
                    }
                },
                {
                    "Name": "MinHumidity",
                    "DisplayName": "Minimum Humidity",
                    "Description": "",
                    "Type": {
                        "Name": "int"
                    }
                },
                {
                    "Name": "MaxHumidity",
                    "DisplayName": "Maximum Humidity",
                    "Description": "",
                    "Type": {
                        "Name": "int"
                    }
                },
                {
                    "Name": "MinTemperature",
                    "DisplayName": "Minimum Temperature",
                    "Description": "",
                    "Type": {
                        "Name": "int"
                    }
                },
                {
                    "Name": "MaxTemperature",
                    "DisplayName": "Maximum Temperature",
                    "Description": "",
                    "Type": {
                        "Name": "int"
                    }
                },
                {
                    "Name": "ComplianceSensorType",
                    "DisplayName": "Compliance Sensor Type",
                    "Description": "",
                    "Type": {
                        "Name": "enum",
                        "EnumValues": ["None","Humidity","Temperature"]
                    }
                },
                {
                    "Name": "ComplianceSensorReading",
                    "DisplayName": "Compliance Sensor Reading",
                    "Description": "",
                    "Type": {
                        "Name": "int"
                    }
                },
                {
                    "Name": "ComplianceStatus",
                    "DisplayName": "Compliance Status",
                    "Description": "",
                    "Type": {
                        "Name": "bool"
                    }
                },
                {
                    "Name": "ComplianceDetail",
                    "DisplayName": "Compliance Detail",
                    "Description": "",
                    "Type": {
                        "Name": "string"
                    }
                },
                {
                    "Name": "LastHumidity",
                    "DisplayName": "Last Humidity",
                    "Description": "",
                    "Type": {
                        "Name": "int"
                    }
                },
                {
                    "Name": "LastTemperature",
                    "DisplayName": "Last Temperature",
                    "Description": "",
                    "Type": {
                        "Name": "int"
                    }
                },
                {
                    "Name": "LastSensorUpdateTimestamp",
                    "DisplayName": "Last Sensor Update Timestamp",
                    "Description": "",
                    "Type": {
                        "Name": "int"
                    }
                }
    ```

4.  **"Constructor"** オブジェクト内の次のメタデータJSONを変更します。 これにより、Blockchain Workbench へのスマートコントラクトのコンストラクタのメタデータ定義が提供されます。

    **TelemetryCompliance** コントラクトには、そのコンストラクター用の7つのパラメーターがあります。 また、3つのプロパティ（*device*、*supplyChainOwner*、*supplyChainObserver*）が "*user*"型として指定されています。 これにより、Blockchain Workbenchがダウンストリームのクライアントおよびシステムに提供できるコンテキストが提供されます。

    このコントラクトでは、2種類のセンサー(温度と湿度)からテレメトリを評価することによって、環境内で品目を維持している取引先のコンプライアンスを測定しています。 コンストラクターの残りの4つのパラメーターは、準拠して考慮されることが許可されているセンサー読み取り値の最小および最大許容範囲（*minHumidity*、*maxHumidity*、*minTemperature*、*maxTemperature*）を取り込みます。


    ```json
            "Constructor": {
                "Parameters": [

                    {
                        "Name": "device",
                        "Description": "...",
                        "DisplayName": "Device",
                        "Type": {
                            "Name": "user"
                        }
                    },
                    {
                        "Name": "supplyChainOwner",
                        "Description": "...",
                        "DisplayName": "SupplyChain Owner",
                        "Type": {
                            "Name": "user"
                        }
                    },
                    {
                        "Name": "supplyChainObserver",
                        "Description": "...",
                        "DisplayName": "SupplyChain Observer",
                        "Type": {
                            "Name": "user"
                        }
                    },
                    {
                        "Name": "minHumidity",
                        "Description": "...",
                        "DisplayName": "Minimum Humidity",
                        "Type": {
                            "Name": "int"
                        }
                    },
                    {
                        "Name": "maxHumidity",
                        "Description": "...",
                        "DisplayName": "Maximum Humidity",
                        "Type": {
                            "Name": "int"
                        }
                    },
                    {
                        "Name": "minTemperature",
                        "Description": "...",
                        "DisplayName": "Minimum Temperature",
                        "Type": {
                            "Name": "int"
                        }
                    },
                    {
                        "Name": "maxTemperature",
                        "Description": "...",
                        "DisplayName": "Maximum Temperature",
                        "Type": {
                            "Name": "int"
                        }
                    }
                
                ]

            },
    ```

5. スマートコントラクトの*関数*を定義する**Functions**オブジェクトに次のメタデータJSONを追加します。

    今まで設定してきたスマートコントラクトで見たように、今回は5つの機能があります： "*IngestTelemetry*"、 "*RequestTransferResponsibility*"、 "*AcceptTransferResponsibility*"、 "*TakeFinalDelivery*"、および "*Complete*"。

    このラボ演習の前半で見たように、"*user*" のタイプは、アクションに必要なアドレスのタイプについてBlockchain Workbenchにコンテキストを提供するために割り当てられています。

    関数にパラメータがない場合、パラメータは中括弧で表されます。

    ```json
           "Functions": [

                {
                    "Name": "IngestTelemetry",
                    "DisplayName": "Ingest Telemetry",
                    "Description": "...",
                    "Parameters": [
                        {
                            "Name": "humidity",
                            "Description": "...",
                            "DisplayName": "humidity",
                            "Type": {
                                "Name": "int"
                            }
                        },
                        {
                            "Name": "temperature",
                            "Description": "...",
                            "DisplayName": "temperature",
                            "Type": {
                                "Name": "int"
                            }
                        },
                        {
                            "Name": "timestamp",
                            "Description": "...",
                            "DisplayName": "timestampt",
                            "Type": {
                                "Name": "int"
                            }
                        }
                    ]
                },
                {
                    "Name": "RequestTransferResponsibility",
                    "DisplayName": "Request Transfer Responsibility",
                    "Description": "...",
                    "Parameters": [
                        {
                            "Name": "newCounterparty",
                            "Description": "...",
                            "DisplayName": "newCounterparty",
                            "Type": {
                                "Name": "user"
                            }
                        }
                    ]
                },
                {
                    "Name": "AcceptTransferResponsibility",
                    "DisplayName": "AcceptTransferResponsibility",
                    "Description": "...",
                    "Parameters": [
                    ]
                },
                {
                    "Name": "TakeFinalDelivery",
                    "DisplayName": "TakeFinalDelivery",
                    "Description": "...",
                    "Parameters": [
                    ]
                },
                {
                    "Name": "Complete",
                    "DisplayName": "Complete",
                    "Description": "...",
                    "Parameters": [
                    ]
                }
            ]
    ```

6.  スマートコントラクトの*機能*を定義する "**States**"オブジェクトに次のメタデータJSONを追加します。

    ```json
            "States": [

                {
                    "Name": "Creating",
                    "DisplayName": "Creating",
                    "Description": "...",
                    "PercentComplete": 0,
                    "Value": 0,
                    "Style": "Success",
                    "Transitions": [
                    ]
                },
                {
                    "Name": "Created",
                    "DisplayName": "Created",
                    "Description": "...",
                    "PercentComplete": 10,
                    "Value": 1,
                    "Style": "Success",
                    "Transitions": [
                        {
                            "AllowedRoles": ["Admin","User"],
                            "AllowedInstanceRoles": ["InitiatingCounterparty"],
                            "Description": "...",
                            "Function": "RequestTransferResponsibility",
                            "NextStates": [ "InTransit" ],
                            "DisplayName": "RequestTransferResponsibility"
                        },
                        {
                            "AllowedRoles": ["Admin","User"],
                            "AllowedInstanceRoles": ["Device"],
                            "Description": "...",
                            "Function": "IngestTelemetry",
                            "NextStates": [ "InTransit", "OutOfCompliance" ],
                            "DisplayName": "IngestTelemetry"
                        }
                    ]
                },
                {
                    "Name": "TransitionRequestPending",
                    "DisplayName": "TransitionRequestPending",
                    "Description": "...",
                    "PercentComplete": 50,
                    "Value": 2,
                    "Style": "Success",
                    "Transitions": [
                        {
                            "AllowedRoles": ["Admin","User"],
                            "AllowedInstanceRoles": ["Counterparty"],
                            "Description": "...",
                            "Function": "AcceptTransferResponsibility",
                            "NextStates": [ "InTransit" ],
                            "DisplayName": "AcceptTransferResponsibility"
                        },
                        {
                            "AllowedRoles": ["Admin","User"],
                            "AllowedInstanceRoles": ["Device"],
                            "Description": "...",
                            "Function": "IngestTelemetry",
                            "NextStates": [ "InTransit", "OutOfCompliance" ],
                            "DisplayName": "IngestTelemetry"
                        }
                    ]
                },
                {
                    "Name": "InTransit",
                    "DisplayName": "InTransit",
                    "Description": "...",
                    "PercentComplete": 50,
                    "Value": 3,
                    "Style": "Success",
                    "Transitions": [
                        {
                            "AllowedRoles": ["Admin","User"],
                            "AllowedInstanceRoles": ["Counterparty"],
                            "Description": "...",
                            "Function": "RequestTransferResponsibility",
                            "NextStates": [ "FinalDelivery" ],
                            "DisplayName": "RequestTransferResponsibility"
                        },
                        {
                            "AllowedRoles": ["Admin","User"],
                            "AllowedInstanceRoles": ["Counterparty"],
                            "Description": "...",
                            "Function": "TakeFinalDelivery",
                            "NextStates": [ "FinalDelivery" ],
                            "DisplayName": "TakeFinalDelivery"
                        },
                        {
                            "AllowedRoles": ["Admin","User"],
                            "AllowedInstanceRoles": ["Device"],
                            "Description": "...",
                            "Function": "IngestTelemetry",
                            "NextStates": [ "FinalDelivery", "OutOfCompliance" ],
                            "DisplayName": "IngestTelemetry"
                        }
                    ]
                },
                {
                    "Name": "FinalDelivery",
                    "DisplayName": "FinalDelivery",
                    "Description": "...",
                    "PercentComplete": 90,
                    "Value": 4,
                    "Style": "Success",
                    "Transitions": [
                        {
                            "AllowedRoles": ["Admin","User"],
                            "AllowedInstanceRoles": [],
                            "Description": "...",
                            "Function": "Complete",
                            "NextStates": [ "Completed" ],
                            "DisplayName": "Complete"
                        }
                    ]
                },
                {
                    "Name": "Completed",
                    "DisplayName": "Completed",
                    "Description": "...",
                    "PercentComplete": 100,
                    "Value": 5,
                    "Style": "Success",
                    "Transitions": [
                    ]
                },
                {
                    "Name": "OutOfCompliance",
                    "DisplayName": "OutOfCompliance",
                    "Description": "...",
                    "PercentComplete": 100,
                    "Value": 6,
                    "Style": "Failure",
                    "Transitions": [
                    ]
                }

            ]
    ```

7. ファイルを保存します。


### タスク3：スマートコントラクトを展開する

1. 新しいブラウザ/タブを開き、**Blockchain Workbench Web Client**アプリケーションに移動します。

2. Azure Subscriptionにログインした、以前にアプリの*管理者*として構成されていたのと同じアカウントを使用して、Blockchain Workbench Webアプリケーションにログインします。

3. 新しいブロックチェーンアプリケーションの作成を開始するには、**+New**タイルボタンをクリックします。

    ![The New button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image87.png "Create a new Blockchain application")

    > **Note**：+ Newタイルボタンが表示されない場合は、戻って管理者ユーザー権限が正しく設定されていることを確認してください。

4.  **New Application**フォームで、**Browse**ボタンをクリックします。

    ![The Browse button for uploading the contract configuration json is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image88.png "Upload contract configuration json")

5. **Open**ダイアログで**TelemetryCompliance.json**ファイルを探して選択し、**Open**をクリックします。

    ![The TelemetryCompliance.json file is highlighted within the HOL folder.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/open-file-telemetrycompliance-json.png "Select the TelemetryCompliance.json file")

    > **Note**：JSONファイルをアップロードするとき、検証がかかります。 ファイルにエラーがある場合は、**Browse**ボタンのすぐ下にある赤いボックスに表示されます。 Azure Blockchain WorkbenchのJSONスキーマについて質問がある場合は、次のURLを参照してください。 [Azure Blockchain Workbench configuration reference](https://docs.microsoft.com/en-us/azure/blockchain-workbench/blockchain-workbench-configuration-overview)
    
    >![Errors in the uploaded json file are displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/telemetrycompliance-json-file-errors-display-screenshot.png "Errors in the uploaded json file are displayed")
    
     
    >以下のツールを使ってファイルのエラーをチェックすることもできます。
    >  - <http://remix.ethereum.org> から入手できる remix IDE を使用して Solidity（.sol）スマートコントラクトのソースコードをテストします。
    >
    >  - <http://jsoneditoronline.org>を使用してJSONファイルをテストします。

6. **Browse**ボタンをクリックして**コントラクトのコード（.sol、.zip）**をアップロードします。スマートコントラクト の**TelemetryCompliance.sol** Solidityソースコードファイルを選択します。
    
    ![The Browse button for uploading the contract code is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image89.png "Uplaod the contract code")

7. アップロードされたファイルのファイル検証に合格したら、**デプロイ**をクリックしてブロックチェーンアプリケーションをAzure Blockchain Workbenchにデプロイします。

    ![The Deploy button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/blockchain-workbench-deploy-application-button.png "The Deploy button is highlighted")

8. デプロイが完了すると、アプリケーションのリストに**Telemetry Compliance**ブロックチェーンアプリケーションが表示されます。

    ![The newly created Blockchain application is displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/blockchain-application-deployment-success.png "The newly created Blockchain application is displayed")

## 課題5：コントラクトペルソナへのユーザーの割り当て

この演習では、ブロックチェーンソリューション内のさまざまなユーザーロールに対応するAzure AD内に追加のユーザーをいくつか作成します。 これらのユーザーは、Azure Blockchain Workbench内のさまざまなペルソナにも割り当てられ、ワークフロー内での役割は一致します。

### タスク1：Azure AD でユーザーを作成する

1. Azure ポータルに再度アクセスします。

2. ディレクトリが正しいか確認します

    ![The Directory + Subscription filter button is highlighted in the top toolbar of the Azure Portal.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image58.png "The Directory + Subscription filter button is highlighted in the top toolbar of the Azure Portal")

3. Azureポータルの左側にあるナビゲーションで、**Azure Active Directory**をクリックします。

    ![Azure Active Directory menu item is displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image98.png "Azure Active Directory menu item is displayed")

4. **Azure Active Directory**ブレードで、**Users**をクリックします。

    ![Users link is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image99.png "Users link is highlighted")

5. 新しいユーザーの追加を開始するには、**+New user**をクリックします。

    ![New user button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image100.png "New user button is highlighted")

6. **User**ブレードで、以下の値を入力します。

    a.  Name: **Woodgrove Distribution**

    b.  User name: **woodgrovedistribution**@\<your-azure-ad-tenant\>.onmicrosoft.com.

    ![The Name and User name fields above are set.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image101.png "The Name and User name fields above are set")

7. **パスワードの表示** チェックボックスをオンにしてから、後で参照できるようにパスワードをコピーしてください。

    ![The Password is displayed and the Show Password checkbox is selected and highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image102.png "Display and copy the password")

8. **Create**をクリックします

    ![Click the Create button.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image103.png "Click the Create button")

9. 手順を繰り返してAzure AD内に次の追加のユーザーを作成し、それらのユーザーと共にBlockchain Workbench Webクライアントにログインして、アカウントを作成します。

    | Name                              | User name
    | --------------------------------- | ---------------------------------
    | Contoso Shipping                  | **contososhipping**@\<your-azure-ad-tenant\>.onmicrosoft.com
    | Blockchain Shipping               | **blockchainshipping**@\<your-azure-ad-tenant\>.onmicrosoft.com
    | Simulated Device                  | **simulateddevice**@\<your-azure-ad-tenant\>.onmicrosoft.com
    | Northwind Traders Supplychain     | **northwindtraderssupplychain**@\<your-azure-ad-tenant\>.onmicrosoft.com
    | Government Regulator              | **governmentregulator**@\<your-azure-ad-tenant\>.onmicrosoft.com

    >**Note**: Be sure to record the passwords for each account so you can login with them later.

10. ここまでの設定で、Azure ADディレクトリ内には合計7人のユーザー**がいるはずです。 追加した6に加えて、Azure ADテナントとAzureサブスクリプションを管理するためのMicrosoftアカウントです。

    ![The list of all users creates is displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image108.png "The list of all users creates is displayed")

### タスク2：ユーザ割り当ての作成

1. シークレットモードまたはプライベートブラウザモードで新しいブラウザウィンドウを開き、**Blockchain Client App**のURLに移動します。

2. プロンプトが表示されたら、Azure AD内の**Administrator**グループに以前に追加した**Microsoftアカウント**を使用してサイトにログインします。

    ![Login with Microsoft Account.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image66.png "Login with Microsoft Account")

3. ログインしたら、**Telemetry Compliance**アプリケーションをクリックしてください。

    ![The newly created Blockchain application is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/clientapp-applications-list-telemetry-compliance-app.png "The newly created Blockchain application is highlighted")

4. **Telemetry Compliance**アプリケーションで、右上の**members**リンクをクリックしてください。

    ![The count of how many members are added to the Blockchain application is highlighted and shows zero.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/clientapp-telemetry-compliance-app-zero-members-link.png "The count of how many members are added to the Blockchain application is highlighted and shows zero")

5. **Membership**ペインで、**Add a member**ボタンをクリックします。

    ![Add a member button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/workbench-membership-add-member-button.png "Add a member button is highlighted")

6. **Add a member**ペインの**Northwind Traders Supplychain**をアカウント名フィールドに入力し、をクリックしてAzure AD内の推奨されるユーザーの一覧からそのユーザーを選択します。

    ![The user is entered in the field.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/workbench-add-member-name.png "The user is entered in the field")

7. **User**の**Role**を選択し、次に**Add**をクリックします。

    ![The member type of User is selected, and the Add button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/workbench-add-member-role.png "Set the member type")

8. 上記の手順を繰り返して、次のメンバーユーザーのリストを追加します。

    | Member User Name               | Role |
    | ----------------               | ---- |
    | Woodgrove Distribution         | User |
    | Contoso Shipping               | User |
    | Blockchain Shipping            | User |
    | Simulated Device               | User |
    | Northwind Traders Supplychain  | User |
    | Government Regulator           | Auditor |

9. ここまでで**6人のMember Users**が追加されているはずです。

    ![The full list of members added to the Blockchain application is displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/workbench-members-list.png "The full list of members added to the Blockchain application is displayed")

## 課題6：スマートコントラクトのインスタンスを作成して処理する

この課題では、はTelemetryComplianceスマートコントラクトの新しいインスタンスを作成し、スマートコントラクトのコードでプログラムされているようにサプライチェーンのワークフローを進めます。 また、 "Simulated Device"ユーザーとしてログインしてIoT Device Telemetryをシミュレートすることで、Complianceをテストします。


### タスク1：コントラクトインスタンスの作成

1. シークレットモードまたはプライベートブラウザモードで新しいブラウザを開き、**Blockchain Workbench Web Client URL**に移動します。

    > **Note**：すべての異なるユーザーアカウントを作成し終えたので、Blockchainネットワークの各アカウントを設定するには、ログインごとにBlockchain Workbench Webアプリケーションに必ずログインする必要があります。 また、これらのアカウントは作成されたばかりなので、このタスクを続行する前に、その設定がブロックチェーンネットワーク内で完全に処理を完了するまで数分待つ必要があります。

2.  **woodgrovedistribution**ユーザーとしてログインします。

    ![Login with the Azure AD account.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image123.png "Login with the Azure AD account")

3.  **Telemetry Compliance**アプリケーションタイルをクリックしてください。

    ![The tile for the Telemetry Compliance application is displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image124.png "The tile for the Telemetry Compliance application is displayed")

4.  **New Contract**ボタンをクリックしてください。

    ![The New Contract button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image125.png "The New Contract button is highlighted")
    
5.  **Create**ダイアログで、以下の値を入力します。

    a.  device: **Simulated Device**

    b.  supplyChainOwner: **Northwind Traders Supplychain**

    c.  supplyChainObserver: **Government Regulator**

    d.  minHumidity: **80**

    e.  maxHumidity: **90**

    f.  minTemperature: **40**

    g.  maxTemperature: **55**

    ![The above values are set.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image126.png "The above values are set")

6.  **Create**をクリックします

7.  コントラクトが作成されるのを待ちます。

    ![Working on contract message is displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image127.png "Working on contract message is displayed")

    
    > **Note**：新しいコントラクトを作成するのに数分かかることがあります。

8.  コントラクトが作成されたら、リスト内の新しいコントラクトをクリックします。

    ![The new contract displayed in the list with a State of Created.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image128.png "New contract has been created")

9.  スマートコントラクトを表示しているときに、下にスクロールして**Status**および**Details**ペインを表示します。 **Status**が**Created**と表示されていることに注目してください。

    ![The details of the contact are displayed with the current Status highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image129.png "The Contract is in the Created state")

10. ページの一番上までスクロールして、**アクションを実行する**ボタンをクリックします。

    ![The Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image131.png "The Take action button is highlighted")

11. **Action**ドロップダウンで、**Request Transfer Responsibility**を選択し、次に**Take action**をクリックします。

    ![The Request Transfer Responsibility action is selected and highlighted, and the Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image132.png "The Request Transfer Responsibility action is selected and highlighted, and the Take action button is highlighted")

12. **newCounterparty**フィールドに**Contoso Shipping**ユーザーを入力してから、**Take action**をクリックします。

    ![The newCounterpart is set to Contoso Shipping, and the Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/request-transfer-responsibility-newCounterparty.png "The newCounterpart is set to Contoso Shipping, and the Take action button is highlighted")

13. スマートコントラクトの**Actions**ペインに、"*You took action Request Transfer Responsibility*".というメッセージが表示されていることを確認します。

    ![Message displays that action has been taken.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/request-transfer-responsibility-you-took-action.png "Message displays that action has been taken")

14. ログアウトしてブラウザを閉じます。

### タスク2：Contoso Shippingへの責任の移管

1. シークレットモードまたはプライベートブラウザモードで新しいブラウザを開き、**Blockchain Workbench WebクライアントのURL**に移動します。

2. **contososhipping**ユーザーとしてログインします。

    ![Login with Azure AD account.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image133.png "Login with Azure AD account")

3. **Telemetry Compliance**アプリケーションタイルをクリックしてください。

    ![Telemetry Compliance application tile is displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image124.png "Telemetry Compliance application tile is displayed")

4. リストの**Smart Contract**をクリックしてください。

    ![Smart contract is displayed in the list.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image135.png "Smart contract is displayed in the list")

5. **Status**セクションまでスクロールして、Statusが現在**TransitionRequestPending**に設定されていることを確認します。

    ![Current status of TransitionRequestPending is displayed for the contract.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image136.png "Current status of TransitionRequestPending is displayed for the contract")

6. ページの一番上までスクロールして、**Take action**ボタンをクリックします。

    ![Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image137.png "Take action button is highlighted")

7. Actions のドロップダウンで、**AcceptTransferResponsibility**を選択し、**Take action **をクリックします。

    ![The AcceptTransferResponsibility action is selected and highlighted, and the Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image138.png "The AcceptTransferResponsibility action is selected and highlighted, and the Take action button is highlighted")

8. ブロックチェーン内で処理が移管されるまで1、2分待ちます。

9. **Status**セクションまでスクロールして、Statusが**InTransit**に設定されていることを確認します。


    ![The current state of the contract is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image139.png "The current state of the contract is highlighted")

10. 一番上までスクロールして、**Take action** ボタンをクリックします。

    ![Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image142.png "Take action button is highlighted")

11. Actions ドロップダウンで、**Transfer Responsibility**を選択し、**Take action**をクリックします。

    ![The Request Transfer Responsibility action is selected.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image140.png "The Request Transfer Responsibility action is selected")

12. **newCounterparty**フィールドで、**Blockchain Shipping**ユーザーを選択してから、**Take action**をクリックします。

    ![The newCounterparty is set and the Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image143.png "The newCounterparty is set and the Take action button is highlighted")

13. ログアウトしてブラウザを閉じます。

### タスク3：シミュレートされたデバイスのテレメトリを取り込む

1.  Open a new browser in Incognito or Private browser mode, and navigate to the **Blockchain Workbench Web Client URL**.

2.  シークレットモードまたはプライベートブラウザモードで新しいブラウザを開き、**Blockchain Workbench Web Client URL**に移動します。

    ![Login with Azure AD account.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image144.png "Login with Azure AD account")

3.  **Telemetry Compliance**アプリケーションタイルをクリックしてください。

    ![Application tile is displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image124.png "Application tile is displayed")

4.  リストの**スマートコントラクト**をクリックしてください。

    ![Smart contract is displayed in the list.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image145.png "Smart contract is displayed in the list")

5.  **Take action** ボタンをクリックします

    ![Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image142.png "Take action button is highlighted")

6. Actions ドロップダウンから**Ingest Telemetry**を選択し、**Take action**をクリックします。

    ![The Ingest Telemetry action is selected, and the Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image146.png "The Ingest Telemetry action is selected, and the Take action button is highlighted")

7. **IngestTelemetry** ダイアログで、以下の値を入力します。

    a.  Humidity: **85**

    b.  Temperature: **48**

    ![The above values are set.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image147.png "The above values are set")

8. **timestamp**フィールドは、**Unix タイムスタンプ**として入力する必要があります。 現在のUnixタイムスタンプを簡単に取得するには、新しいブラウザを開いて<http://www.unixtimestamp.com>に移動し、**Current Unix Timestamp**をコピーして、その中の**timestamp**フィールドに貼り付けます。 **IngestTelemetry**ダイアログ

    ![The current Unix timestamp is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image148.png "The current Unix timestamp is highlighted")

9. **タイムスタンプ**を入力したら、**Take action**をクリックします。


    
    ![The Ingest Telemetry values are entered.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image149.png "The Ingest Telemetry values are entered")

10. スマートコントラクトインスタンスが追跡しているパッケージの**Simulated Device**は、ログに記録され、スマートコントラクトで検証されました。

11. ログアウトしてブラウザを閉じます。

### タスク4：ブロックチェーン シッピングの責任を引き受ける

1. シークレットモードまたはプライベートブラウザモードで新しいブラウザを開き、**Blockchain Workbench Web Client URL**に移動します。

2. **blockchainshipping**ユーザーとしてログインします。

    ![Login with Azure AD account.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image150.png "Login with Azure AD account")

3. **Telemetry Compliance**アプリケーションタイルをクリックしてください。

    ![The application tile is displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image124.png "The application tile is displayed")

4. **スマートコントラクト**をクリックしてください。

    ![The contract is displayed in the list.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image135.png "The contract is displayed in the list")

5. **Take action** ボタンをクリックします

    ![Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image142.png "Take action button is highlighted")

6. Actions ドロップダウンから**AcceptTransferResponsibility**を選択し、**Take action**をクリックします。

    ![The AcceptTransferResponsibility action is selected, and the Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image152.png "The AcceptTransferResponsibility action is selected, and the Take action button is highlighted")

7. ブロックチェーン内でアクションが完了するまで1〜2分待ちます。

8. **Take action** ボタンをクリックします

    ![Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image142.png "Take action button is highlighted")

9. Actions ドロップダウンから**Request Transfer Responsibility**を選択し、**Take action**をクリックします。

    ![The Request Transfer Responsibility action is selected, and the Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image155.png "The Request Transfer Responsibility action is selected, and the Take action button is highlighted")

10. **newCounterparty**フィールドで、**Northwind Traders Supplychain**ユーザーを選択してから、**Take action**をクリックします。

    ![The newCounterparty is set, and the Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image156.png "The newCounterparty is set, and the Take action button is highlighted")

11. ログアウトしてブラウザを閉じます。

### タスク5：Northwindトレーダーへの最終配信

1. シークレットモードまたはプライベートブラウザモードで新しいブラウザを開き、**Blockchain Workbench Web Client URL**に移動します。

2. **northwindtraderssupplychain**ユーザーとしてログインします。

    ![Login with Azure AD account.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image157.png "Login with Azure AD account")

3. **Telemetry Compliance**アプリケーションタイルをクリックしてください。

    ![The application tile is displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image124.png "The application tile is displayed")

4. **Take action** ボタンをクリックします

    ![Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image142.png "Take action button is highlighted")

5. Actions ドロップダウンから**AcceptTransferResponsibility**を選択し、**Take action**をクリックします。

    ![The AcceptTransferResponsibility action is selected, and the Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/take-action-accept-transfer-responsibility.png "The AcceptTransferResponsibility action is selected, and the Take action button is highlighted")

6. ブロックチェーン内でアクションが完了するまで1〜2分待ちます。

7. **Take action** ボタンをクリックします

    ![Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image142.png "Take action button is highlighted")

8. Actions ドロップダウンから**TakeFinalDelivery**を選択し、**Take action**をクリックします。

    ![The TakeFinalDelivery action is selected, and the Take action button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/take-action-take-final-delivery.png "The TakeFinalDelivery action is selected, and the Take action button is highlighted")

9. ブロックチェーン内でアクションが完了するまで1〜2分待ちます。

10. ログアウトしてブラウザを閉じます。

### タスク6：コンプライアンスのためのスマートコントラクトの監査

1. シークレットモードまたはプライベートブラウザモードで新しいブラウザを開き、**Blockchain Workbench Web Client URL**に移動します。

2. **Governmentregulator**ユーザーとしてログインします。

    ![Login with Azure AD account.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image164.png "Login with Azure AD account")

3. **Telemetry Compliance**アプリケーションタイルをクリックしてください。

    ![The application tile is displayed.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image124.png "The application tile is displayed")

4. **スマートコントラクト**をクリックしてください。

    ![The contract is displayed in the list.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image135.png "The contract is displayed in the list")

5. 下にスクロールして、スマートコントラクトの**Status**、**Details**、および**Activity**セクションに表示されている情報を表示します。

    ![The Status section is highlighted and labeled 1, and the Details section is highlighted and labeled 2, and the Activity section is highlighted and labeled 3.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/audit-smart-contract-status.png "View the Smart Contract Status, Details, and Activity")

6. ログアウトしてブラウザを閉じます。

## ハンズオン後

### タスク1：リソースの削除

1. ラボが完成したので、先に進み、このラボ用に作成されたすべてのリソースグループを削除します。 これらのリソースは必要なくなり、Azure Subscriptionをクリーンアップすると効果的です。

### (オプショナル)タスク2：Azure ADテナントを削除する

    >**注:**このラボで使用するためだけに新しいAzure ADテナントを作成した場合にのみ、このタスクに従ってください。 このラボのユーザーアカウントを管理するために既存のAzure ADテナントを使用した場合は、削除しないでください。

1. ブラウザのウィンドウ/タブを開き、<http://portal.azure.com>でAzureポータルに移動してログインします。

2. Azure Portalのトップメニューにある**Directory and Subscription filter**ボタンをクリックして、該当の Azure ADディレクトリを選択します。

    ![The Directory and Subscription filter menu is open with the Northwind Traders Azure AD tenant is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image168.png "Select the Northwind Traders Azure AD tenant int he Directory and Subscription filter menu")

3. 左側のナビゲーションペインで、**Azure Active Directory**をクリックします。

    ![Azure Active Directory is highlighted in the left-side navigation for the Azure Portal.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image169.png "Azure Active Directory is highlighted in the left-side navigation for the Azure Portal")

4. Azure ADテナントを削除する前に、まずクリーンアップする必要があります。

5. Azure Active Directoryブレードで、**Users**をクリックします。

    ![Users is selected under the Manage section.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image170.png "Click on the Users link under the Manage section")

6. このラボ用に作成された各ユーザーを調べて**削除**します。

    ![In the list of Users, all the users that were created during this lab are selected.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image172.png "Each of the Users for this lab are selected for deletion")

7. **Azure Active Directory**ブレードで、**App registrations**をクリックしてください。

    ![App registrations is selected under the Manage section.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image174.png "App registrations is selected under the Manage section")

8. すべてのアプリケーションを表示するには、**View all application**ボタンをクリックします。

9. **Azure Blockchain Workbench Web Client**をクリックしてください。

    ![The Azure Blockchain Workbench Web Client application is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image175.png "The Azure Blockchain Workbench Web Client application is highlighted")

10. **delete**ボタンをクリックし、**Yes**をクリックしてアプリの削除を確認します。

    ![The Delete button and the Yes button are highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image176.png "The Delete button and the Yes button are highlighted")

11. **Azure Active Directory**ブレードで、**Delete directory**ボタンをクリックし、**Delete directory '該当ディレクトリ'？** 確認ウィンドウで**Delete**ボタンをクリックします。

    ![The Delete directory button is highlighted.](https://yuhattor.blob.core.windows.net/share/blockchainhol-images/lab-guide/image178.png "The Delete directory button is highlighted")

