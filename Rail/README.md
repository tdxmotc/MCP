# TDX MCP-臺鐵與高鐵

TDX臺鐵與高鐵MCP服務，整合TDX平臺所提供的臺鐵與高鐵查詢車次、票價與導訂票等API。本MCP 服務的重點並非在於直接對終端用戶提供應用服務，而是著重於驗證 TDX API 與資料已具備AI ready的開放性、標準化、機器易讀等特性，並能輕鬆與 AI 技術整合。

## 功能介紹

- 整合TDX臺鐵與高鐵API服務，提供查詢車次、票價與導訂票。
- 使用Python開發符合MCP標準之服務。
- 相容於所有支援MCP的應用程式。
- TDX MCP服務執行於Server端，Client端透過HTTP連線至MCP服務。
- MCP服務使用會員帶入的API金鑰呼叫TDX API，並納入點數計算。
- 為了降低token使用量，僅回傳部分重要欄位資訊，且每次最多回傳3筆資料。

## 驗證環境

使用Claude Desktop 1.1.6046版本、Sonnet 4.6模型做MCP功能展示。所有回答內容皆由AI產生，難免會產生MCP工具使用時機誤判、回答錯誤訊息的情況，使用不同的MCP Client(如Cursor、Cline、VS Code等)與大語言模型也將產生不同的回應結果。

## 步驟說明

1. 確認本機電腦已安裝`npx` 。
2. 使用npm安裝`mcp-remote` 套件。

```
npm i mcp-remote
```

1. 本機電腦安裝支援整合MCP服務的桌面應用程式，如Claude Desktop。
2. 加入TDX會員，取得API Key(包含Client_ID和Client_Secret)。
3. Claude Desktop設定檔加入MCP設定。

## Claude Desktop

使用Claude Desktop驗證TDX雙鐵MCP服務。

> [!TIP]
> 您可挑選自己喜歡的AI應用程式，但需先確認該程式有支援整合MCP。
> 不同的應用程式有不同的MCP引入與設定方式，詳情請參閱應用程式的官網。


1. 開啟Claude Desktop MCP設定檔。

```markdown
%APPDATA%\Roaming\Claude\claude_desktop_config.json
```

2. 加入MCP服務運作設定。

```markdown
{
  "mcpServers": {
    "tdx_rail_http": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://tdx.transportdata.tw/tdx-mcp/rail",
        "--header",
        "cid:${TDX_CLIENT_ID}",
        "--header",
        "cst:${TDX_CLIENT_SECRET}"
      ],
      "env": {
        "TDX_CLIENT_ID": "{TDX會員CLIENT_ID}",
        "TDX_CLIENT_SECRET": "{TDX會員CLIENT_SECRET}"
      }
    }
  }
}
```

3. 關閉Claude Desktop並重新啟動。

> [!TIP]
> 請務完整退出Claude Desktop並重啟，若只單純關閉Claude Desktop視窗則可能無法正確載入MCP工具。

![image.png](images/image_0.png)

4. 重啟Claude Desktop後，開啟Settings→Developer，確認成功引入tdx_rail_http服務且狀態為running，並確認TDX API金鑰(TDX_CLIENT_ID和TDX_CLIENT_SECRET)資訊是否正確。

![image.png](images/image_1.png)

5. 回到提問視窗，點選左下角「+」圖示→Connector→Add from tdx_rail→挑選Tdx basic prompt，使用預先定義好的Prompt，並確認tdx_rail_http為開啟狀態。

> [!TIP]
> 建議使用Prompt以提升問答正確度，若不使用Prompt可能造成MCP工具使用時機的誤判與回傳錯誤的回應訊息。

![image.png](images/image_2.png)

6. 點選「Manage connectors」，開啟Connectors設定，點選「tdx_rail_http」的Configure按鈕。確認所有MCP工具的狀態為「Always allow」或「Needs approval」。

![image.png](images/image_3.png)

## MCP使用範例

以下範例使用Claude Desktop 1.1.6046版本、Sonnet 4.6模型做展示。

> [!TIP]
> 使用其他Client端應用程式與模型可能產生不同的問答結果。

#### ➡️詢問「明天19:00從板橋到桃園的臺鐵列車」

![image.png](images/image_4.png)

#### ➡️進一步詢問指定車次的票價

![image.png](images/image_5.png)

#### ➡️進一步產生指訂車次與車票數的訂票連結(具時效性)

![image.png](images/image_6.png)

#### ➡️點選導訂連結後，開啟臺鐵訂票網站，並自動帶入指定的日期、起訖站、車次與票數

![image.png](images/image_7.png)

#### ➡️詢問「明天09:00從桃園到左營高鐵」

![image.png](images/image_8.png)

#### ➡️進一步詢問指定車次的票價

![image.png](images/image_9.png)

#### ➡️進一產生指定車次、車廂類型、票數的訂票連結(具時效性)

![image.png](images/image_10.png)

#### ➡️點選導訂連結後，開啟高鐵訂票網站，並自動帶入指定的日期、起訖站、車次、車廂類型與票數

![image.png](images/image_11.png)