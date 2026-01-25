# SmartJack - スマートホームリモコン管理アプリケーション

## 📱 プロジェクト概要

**SmartJack** は、複数の家庭やオフィスの赤外線（IR）デバイスを一元管理し、スマートホーム操作をシンプルなUIで実現するFlutterアプリケーションです。

### 🎯 主な特徴

- **マルチホーム対応**: 複数の家/建物を管理可能
- **ルーム管理**: 各家の中で複数のルーム（部屋）を作成・管理
- **カスタムリモコン**: 複数の異なる家電を1つの画面で操作
- **赤外線制御**: エアコン、テレビ、照明などのIRデバイスを完全制御
- **スマートシーン**: 複数のIRコマンドを組み合わせたマクロ機能
- **ハイブリッド通信**: Wi-FiとモバイルデータでのシームレスなIR送信
- **AWS IoT統合**: AWS Amplify + Cognito による認証とMQTT通信

---

## 📂 プロジェクト全体構造

```
lib/ (98個のDartファイル)
├── main.dart                                 # アプリケーションエントリーポイント
├── main_app.dart                             # メイン画面構造（ナビゲーション管理）
├── database_connection_manager.dart          # SQLiteデータベース管理
├── app_selection_manager.dart                # グローバル選択状態管理（家・ルーム）
├── app_color.dart                            # テーマカラー・テーマモード管理
├── amplify_configuration.dart                # AWS Amplify設定
├── auth_localizations.dart                 # 認証画面の日本語化
│
├── communication_manager/                    # 通信管理モジュール
│   ├── communication_manager.dart            # 通信方式自動選択・IR送信
│   ├── mqtt_service.dart                     # AWS IoT MQTT通信
│   ├── aws_communication_service.dart        # AWS REST API通信
│   ├── mdns_service.dart                     # ローカルWi-Fi (mDNS) 通信
│   ├── ble_manager.dart                      # Bluetooth Low Energy管理
│   ├── iot_device_manager.dart               # IoTデバイス情報管理
│   └── iot_device_registration_service.dart  # IoTデバイス登録サービス
│
├── screens/                                  # 全UIスクリーン
│   ├── main_screen/                          # メイン画面（ホーム）
│   │   ├── main_screen.dart                  # ホーム画面の本体
│   │   ├── room_list_section.dart            # ルーム選択UI
│   │   ├── appliance_grid_section.dart       # デバイス（リモコン）グリッド
│   │   ├── main_screen_providers/
│   │   │   └── selected_room_provider.dart   # 選択ルーム状態管理
│   │   └── main_widgets/
│   │       ├── app_drawer.dart               # サイドドロワー（ナビゲーション）
│   │       └── bottom_nav_bar.dart           # ボトムナビゲーションバー
│   │
│   ├── home_management_screen/               # 家管理画面
│   │   ├── home_main_screen.dart             # 家管理の本体
│   │   ├── home_list_state_manager.dart      # 家リスト状態管理（Notifier）
│   │   ├── home_database_process.dart        # 家関連のDB操作
│   │   ├── home_data_models.dart             # Home データモデル
│   │   └── home_ui_components/
│   │       ├── home_list_display.dart        # 家一覧表示
│   │       ├── add_home_dialog.dart          # 家追加ダイアログ
│   │       └── edit_home_dialog.dart         # 家編集ダイアログ
│   │
│   ├── room_management_screen/               # ルーム管理画面
│   │   ├── room_main_screen.dart             # ルーム管理の本体
│   │   ├── room_list_state_manager.dart      # ルームリスト状態管理
│   │   ├── room_database_process.dart        # ルーム関連のDB操作
│   │   ├── room_data_models.dart             # Room データモデル
│   │   └── room_ui_components/
│   │       ├── room_list_display.dart        # ルーム一覧表示
│   │       ├── add_room_dialog.dart          # ルーム追加ダイアログ
│   │       ├── edit_room_dialog.dart         # ルーム編集ダイアログ
│   │       └── device_settings_dialog.dart   # デバイス（Thing名）設定
│   │
│   ├── remote_control_management_screen/     # リモコン管理画面
│   │   ├── remote_main_screen.dart           # リモコン管理の本体（リスト表示）
│   │   ├── remote_control_screen.dart        # リモコン操作画面（ボタンキャンバス）
│   │   ├── remote_list_state_manager.dart    # リモコンリスト状態管理
│   │   ├── remote_control_models.dart        # Remote データモデル
│   │   ├── remote_control_database_process.dart  # リモコンのDB操作
│   │   ├── draggable_button_data_models.dart     # DraggableWidgetData（ボタンモデル）
│   │   ├── categories_constants.dart             # リモコンカテゴリ定数
│   │   ├── canvas_editor_provider.dart           # キャンバス編集状態管理
│   │   ├── ac_status_provider.dart               # エアコン状態管理
│   │   ├── remote_button_templates.dart          # ボタンテンプレート定義
│   │   ├── remote_display_item.dart              # リモコン表示アイテム
│   │   │
│   │   ├── remote_control_ui_components/        # リモコン関連UI部品
│   │   │   ├── remote_list_display.dart         # リモコン一覧表示
│   │   │   ├── empty_remote_list_display.dart   # 空の一覧表示
│   │   │   ├── add_remote_dialog.dart           # リモコン追加ダイアログ
│   │   │   ├── edit_remote_dialog.dart          # リモコン編集ダイアログ
│   │   │   ├── delete_remote_dialog.dart        # リモコン削除ダイアログ
│   │   │   ├── copy_remote_dialog.dart          # リモコンコピーダイアログ
│   │   │   ├── publish_remote_dialog.dart       # リモコン公開ダイアログ
│   │   │   ├── draggable_remote_button.dart     # ドラッグ可能ボタンウィジェット
│   │   │   ├── button_management.dart           # ボタン追加・編集UI
│   │   │   ├── edit_mode_toggle_button.dart     # 編集モード切り替えボタン
│   │   │   ├── color_picker_dialog.dart         # カラーピッカー
│   │   │   ├── icon_selection_widget.dart       # アイコン選択UI
│   │   │   ├── ac_status_section.dart           # エアコン状態表示セクション
│   │   │   └── ac_temperature_widget.dart       # エアコン温度制御ウィジェット
│   │   │
│   │   ├── ac_models/                           # エアコン専用モデル
│   │   │   ├── ac_widget_data.dart              # エアコンウィジェット基底クラス
│   │   │   ├── ac_cycle_button.dart             # エアコンサイクルボタン（モード、風量など）
│   │   │   ├── ac_temperature.dart              # エアコン温度設定
│   │   │   ├── ac_database_manager.dart         # エアコン状態DB管理
│   │   │   └── ac_widget_converter.dart         # ウィジェット変換ユーティリティ
│   │   │
│   │   └── remote_control_data_process/        # リモコンデータ処理
│   │       ├── remote_control_json_model.dart   # JSON←→Model変換
│   │       └── remote_control_cloud_service.dart # クラウド同期サービス
│   │
│   ├── scene_screen/                           # シーン管理画面
│   │   ├── scene_screen.dart                    # シーン管理画面の本体
│   │   ├── scene_state_manager.dart             # シーンリスト状態管理
│   │   │
│   │   ├── scene_ui_components/
│   │   │   ├── trigger_detail_screen.dart       # トリガー詳細設定画面
│   │   │   ├── trigger_type_selection_dialog.dart # トリガー種類選択
│   │   │   ├── edit_scene_dialog.dart           # シーン編集ダイアログ
│   │   │   ├── delete_scene_dialog.dart         # シーン削除ダイアログ
│   │   │   ├── action_selection_screen.dart     # 実行操作選択画面
│   │   │   ├── execute_package_scene_dialog.dart # パッケージシーン実行
│   │   │   │
│   │   │   └── trigger_sections/
│   │   │       ├── trigger_state_models.dart    # トリガー状態モデル
│   │   │       ├── schedule_trigger_section.dart # スケジュールトリガーUI
│   │   │       └── package_trigger_section.dart  # パッケージトリガーUI
│   │   │
│   │   └── services/
│   │       ├── notification_schedule_service.dart  # スケジュール通知管理
│   │       ├── native_schedule_service.dart        # ネイティブスケジュール
│   │       └── scene_executor_service.dart         # シーン実行エンジン
│   │
│   ├── settings_screen/                         # 設定画面
│   │   ├── settings_screen.dart                 # 設定画面の本体
│   │   ├── settings_data_provider.dart          # 設定状態管理（Riverpod）
│   │   ├── settings_database_process.dart       # 設定のDB操作
│   │   │
│   │   └── settings_widgets/
│   │       ├── home_selection_setting.dart      # ホーム選択設定
│   │       ├── handedness_setting.dart          # 利き手設定（左/右）
│   │       ├── theme_color_setting.dart         # テーマカラー設定
│   │       ├── theme_mode_setting.dart          # テーマモード設定（明/暗）
│   │       └── debug_sign_out_setting.dart      # ログアウト（デバッグ）
│   │
│   └── mqtt_test_screen.dart                    # MQTT通信テスト画面（開発用）
│
├── env/                                         # 環境設定
│   ├── env.dart                                 # 環境変数定義
│   └── env.g.dart                               # 自動生成ファイル
│
└── L10n/ (確認済み、多言語対応)
```

---

## 🏗️ アーキテクチャの詳細

### データレイヤー（Database）

#### 主要テーブル構成

| テーブル名 | 説明 | 主要カラム |
|-----------|------|----------|
| `homes` | 家の情報 | id, homename, displayOrder |
| `rooms` | 部屋の情報 | id, home_id, name, thing_name, device_name |
| `remotes` | リモコン情報 | id, name, category, home_id, ac_model |
| `remotewidgets` | リモコンのボタン | id, remote_id, infraredCode, roomId, operationName, x, y |
| `scenes` | シーン情報 | id, name, trigger_conditions, actions, enabled |
| `settings` | アプリ設定 | handedness, themeColor, themeMode, selectedHomeId |
| `ac_status` | エアコン状態 | remote_id, temperature, mode, fan_speed |

#### DatabaseConnectionManager の役割

```dart
class DatabaseConnectionManager {
  Future<void> open()  // DB初期化
  Future<void> _initializeSchema(Database db)  // スキーマ作成・マイグレーション
}

// Riverpod プロバイダで提供
final databaseProvider = Provider((ref) => DatabaseConnectionManager())
```

---

### 状態管理層（State Management）

#### 主要な Riverpod プロバイダ

**グローバル選択状態**
```dart
// 家とルームの現在の選択状態
final appSelectionManagerProvider = StateNotifierProvider<
  AppSelectionManager, 
  AppSelectionState
>

// アプリ状態の一貫性を監視
final appStateValidatorProvider = Provider
```

**ホーム管理**
```dart
final homeDataProvider = FutureProvider<List<Home>>
final homeListProvider = StateNotifierProvider<HomeListNotifier, ...>
```

**ルーム管理**
```dart
final roomDataProvider(int homeId) = FutureProvider<List<Room>>
final roomListProvider = StateNotifierProvider<RoomListNotifier, ...>
```

**リモコン管理**
```dart
final remoteListProvider = StateNotifierProvider<RemoteListNotifier, ...>
final canvasEditorProvider = StateProvider<CanvasEditorState>
```

**シーン管理**
```dart
final sceneListProvider = StateNotifierProvider<SceneListNotifier, ...>
final selectedSceneCategoryProvider = StateProvider<String>  // 'All' など
final filteredSceneListProvider = Provider<List<Map>>
final sceneEditModeProvider = StateProvider<bool>
final selectedSceneIdsProvider = StateProvider<Set<int>>
```

**設定管理**
```dart
final appSettingsProvider = StateNotifierProvider<AppSettingsNotifier, AppSettings>

// AppSettings が保持する状態
class AppSettings {
  final Handedness handedness;      // 左利き/右利き
  final Color themeColor;           // テーマカラー
  final AppThemeMode themeMode;     // ライト/ダーク
}
```

---

### 通信管理層（Communication Manager）

#### ネットワーク状況に応じた自動選択ロジック

```
ユーザーがIRコマンド送信
  ↓
CommunicationManager.sendIrSignal()
  ↓
接続状態をチェック
  ↓
┌─────────────────────────────────────────┐
│ Wi-Fi に接続？                         │
├─────────────────────────────────────────┤
│ Yes → mDNS でマイコンをスキャン       │
│      ├→ 見つかった → HTTP (mDNS)  │
│      └→ 見つからない → AWS MQTT   │
│ No  → AWS MQTT で送信               │
└─────────────────────────────────────────┘
```

**各通信サービスの役割**
- **MdnsService**: ローカルネットワーク内のマイコンをmDNSで検索、HTTP経由でIR送信
- **MqttService**: AWS IoT Core に接続、MQTT でIRコマンドをパブリッシュ
- **AwsCommunicationService**: AWS REST API、デバイス登録・更新

---

### 画面層（UI/Screens）

#### 画面間の遷移フロー

```
MyApp (認証ラッパー)
  └─ MainApp (メイン構造)
     └─ Scaffold
        ├─ AppBar: 現在の画面タイトル
        ├─ Drawer or Drawer (handedness に応じた配置)
        │  └─ ナビゲーションメニュー
        ├─ body: 選択された画面
        │  ├─ HomeScreen (ホーム)
        │  │  ├─ RoomListSection (ルーム選択)
        │  │  └─ ApplianceGridSection (リモコングリッド)
        │  ├─ SceneScreen (シーン管理)
        │  │  ├─ SceneListWidget (シーン一覧)
        │  │  └─ TriggerDetailScreen (詳細設定)
        │  └─ SettingsScreen (設定)
        └─ BottomNavigationBar (ナビゲーション)
```

#### 各画面の主な機能

**HomeScreen**
- 選択中の家・ルームを表示
- ルームの切り替えUI
- リモコンのグリッド表示（最大12個程度）
- リモコンをタップして操作画面へ遷移

**RemoteControlScreen**
- ボタンキャンバス（ドラッグ可能）
- ボタンの追加・削除・編集
- IR学習・送信機能
- エアコン操作（温度、モード、風量）

**SceneScreen**
- シーンリスト表示
- トリガー設定（スケジュール、パッケージ）
- シーン実行・削除
- 通知スケジュール管理

**SettingsScreen**
- テーマカラー・モード変更
- 利き手設定
- ホーム選択
- ログアウト

---

## 🌐 認証画面の日本語化

AWS Amplifyの認証UI（ログイン・サインアップ画面）がデフォルトで英語なため、`auth_localizations.dart` で日本語化を実装しています。

Authenticator ウィジェットに `stringResolver` を渡すことで、ログイン画面のテキストを日本語で表示：

```dart
return Authenticator(
  stringResolver: localizedAuthStringResolver,
  child: MaterialApp(...)
);
```

これにより、以下の画面が日本語化されます：
- ログイン画面
- サインアップ画面
- パスワードリセット画面
- 確認コード入力画面

---

## 📊 データモデル一覧

### Home
```dart
class Home {
  int? id;
  String homename;      // 家の名前
  int displayOrder;     // 表示順
}
```

### Room
```dart
class Room {
  int? id;
  String name;          // 部屋の名前
  int? homeId;          // 所属家
  int? displayOrder;    // 表示順
  String? deviceName;   // mDNS ホスト名
  String? thingName;    // AWS IoT Thing名
}
```

### Remote
```dart
class Remote {
  int? id;
  String name;          // リモコン名
  int displayOrder;     // 表示順
  String? category;     // 'テレビ', 'エアコン', 'カスタム' など
  int? homeId;          // カスタムリモコンのみ指定
  String? acModel;      // エアコン機種名
}
```

### DraggableWidgetData（ボタン）
```dart
class DraggableWidgetData {
  int? id;
  int parentId;         // リモコンID
  String type;          // 'button'
  double x, y;          // キャンバス座標
  String? text;         // ボタンテキスト
  int? buttonColor;     // RGB色
  bool? isSquare;       // 形状（true=正方形, false=円形）
  String? infraredCode; // JSON形式のIRコマンド
  int? roomId;          // カスタムリモコンの場合、操作対象ルームID
  String? operationName;// 操作内容（例：「電源」「温度+」）
}
```

### AppSettings
```dart
class AppSettings {
  Handedness handedness;   // right / left
  Color themeColor;        // テーマカラー
  AppThemeMode themeMode;  // light / dark
}
```

---

## 🔌 通信プロトコル

### IR コマンドの JSON フォーマット

```json
{
  "commands": [
    {
      "protocol": 1,           // 1=NEC, 2=SONY, 3=SAMSUNG など
      "data": "0xFFE01F",      // デコード値（16進）
      "bits": 32,              // ビット数
      "raw": [9000, 4500, ...],// RAWデータ（非標準プロトコルのみ）
      "len": 67                // RAWデータ配列の長さ
    }
  ]
}
```

### MQTT トピック構造

```
{thingName}/ir-command        # IR送信コマンド
{thingName}/ir-response       # 送信結果
```

---

## 🎨 テーマシステム

### AppThemeWidget

- **seedColor**: `themeColor` から自動生成
- **brightness**: `themeMode` に応じてライト/ダーク切り替え
- **TextFieldCursor**: テーマに応じて最適な色
- **白テーマ特例**: `themeColor == Colors.white` の場合、明示的に黒白スキーム

---

## 🔍 主要な処理フロー

### 1. アプリ起動フロー

```
main()
  ↓
WidgetsFlutterBinding.ensureInitialized()
  ↓
Amplify 初期化
  ↓
DatabaseConnectionManager.open()
  ↓
NotificationScheduleService.initialize()
  ↓
ProviderContainer 作成
  ↓
appSelectionManagerProvider.restoreSelection()
  ↓
mqttService.setupSecureIoTConnection()
  ↓
MyApp 実行
```

### 2. ホーム画面の読み込みフロー

```
HomeScreen 構築
  ↓
appSelectionManagerProvider を監視
  ↓
RoomListSection (ルーム選択UI)
  ↓
selectedRoom → remoteListProvider
  ↓
ApplianceGridSection (リモコン表示)
```

### 3. IR送信フロー

```
ユーザーがボタンをタップ
  ↓
RemoteControlScreen.onButtonTapped()
  ↓
infraredCode 取得
  ↓
CommunicationManager.sendIrSignal(hostname, thingname, irData)
  ↓
接続状態判定
  ├─ Wi-Fi + mDNS検出 → MdnsService.sendIrData()
  ├─ Wi-Fi + 未検出 → MqttService.publish()
  └─ モバイルデータ → MqttService.publish()
  ↓
マイコン → IR送信
```

### 4. シーン実行フロー

```
SceneExecutorService.executeScene(sceneId)
  ↓
scene の trigger_conditions をパース
  ↓
actions リスト取得
  ↓
各 action ループ
  ├─ operationId から infraredCode を取得
  ├─ CommunicationManager.sendIrSignal()
  └─ 遅延（複数操作の場合）
  ↓
完了
```

---

## 🛠️ 開発ガイド

### 新しいリモコンカテゴリを追加

1. `lib/screens/remote_control_management_screen/categories_constants.dart` を編集
2. カテゴリ定数を追加
3. UI側でフィルタリング処理を更新

### 新しいシーンのトリガー種別を追加

1. `lib/screens/scene_screen/scene_ui_components/trigger_sections/` に新しいセクションファイルを作成
2. `trigger_type_selection_dialog.dart` にオプションを追加
3. `scene_state_manager.dart` でトリガー条件の保存処理を実装

### エアコン機種対応を追加

1. `lib/screens/remote_control_management_screen/ac_models/` にAC専用モデルを作成
2. `ac_database_manager.dart` で機種別の状態管理を実装
3. `RemoteControlScreen` で機種判定を追加

---

## 📦 依存パッケージ

| パッケージ | バージョン | 用途 |
|-----------|-----------|------|
| `flutter_riverpod` | ^2.5.1 | 状態管理・依存性注入 |
| `sqflite` | ^2.3.0 | SQLiteデータベース |
| `http` | ^1.2.1 | REST API通信 |
| `flutter_blue_plus` | ^1.31.18 | Bluetooth Low Energy |
| `multicast_dns` | ^0.3.2 | mDNS サービス検索 |
| `amplify_flutter` | ^2.7.0 | AWS Amplify基盤 |
| `amplify_auth_cognito` | (auto) | AWS Cognito認証 |
| `amplify_authenticator` | (auto) | 認証UI |
| `path_provider` | ^2.1.3 | ファイルパス取得 |
| `uuid` | ^4.4.0 | ユニークID生成 |
| `flutter_colorpicker` | ^1.1.0 | カラーピッカーUI |

---

## 🧪 テスト構成

```
test/
├── app_color_test.dart
├── app_selection_manager_test.dart
├── database_connection_manager_test.dart
├── main_app_test.dart
├── main_test.dart
├── communication_manager/
└── screens/

integration_test/
├── settings_screen_test.dart
├── home_management_screen/
└── fixtures/
```

---

## ⚙️ セットアップ・実行

### 前提条件
- Flutter 3.9.0 以上
- Dart 3.9.0 以上
- iOS 12.0+ または Android API 21+

### 初期セットアップ

```bash
# 依存パッケージをインストール
flutter pub get

# コード生成（Riverpod, L10n）
flutter pub run build_runner build --delete-conflicting-outputs

# iOS開発の場合
cd ios && pod install && cd ..
```

### 実行

```bash
flutter run
```

---

## 🐛 トラブルシューティング

### ビルドエラー
```bash
flutter clean
flutter pub get
flutter pub run build_runner build --delete-conflicting-outputs
```

### MQTT接続失敗
1. AWS IoT Core 証明書の有効性確認
2. `mqtt_test_screen.dart` で接続テスト
3. ネットワーク接続状況確認

### mDNS未検出
1. Wi-Fiに正しく接続か確認
2. マイコンのmDNSホスト名が正しいか確認
3. ローカルファイアウォールの設定確認

---

## 📝 ライセンス

プロプライエタリソフトウェア

---

## 📞 サポート

問題発生時は以下を確認してください：
- ネットワーク接続状況
- AWS IoT Core の証明書有効性
- Flutter/Dart バージョン
- デバッグログ（debugPrint）出力
