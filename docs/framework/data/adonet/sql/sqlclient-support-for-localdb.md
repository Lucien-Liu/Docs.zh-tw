---
title: LocalDB 的 SqlClient 支援
ms.date: 03/30/2017
ms.assetid: cf796898-5575-46f2-ae6e-21e5aa8c4123
ms.openlocfilehash: 55ab1346de6f5c14f15d01344a984c18edf30e02
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2020
ms.locfileid: "94824476"
---
# <a name="sqlclient-support-for-localdb"></a>LocalDB 的 SqlClient 支援

本文討論如何連接至 LocalDB 資料庫。 LocalDB 是 SQL Server 的輕量版本。
  
## <a name="remarks"></a>備註
  
 摘要說明您可以使用 LocalDB 執行的作業：  
  
- 使用 sqllocaldb.exe 或您的 app.config 檔案來建立及啟動 LocalDB 執行個體。  
  
- 使用 sqlcmd.exe 可在 LocalDB 執行個體中新增和修改資料庫。 例如： `sqlcmd -S (localdb)\myinst` 。  
  
- 使用 `AttachDBFilename` 連接字串關鍵字，將資料庫新增至您的 LocalDB 執行個體。 使用 `AttachDBFilename` 時，如果您沒有使用 `Database` 連接字串關鍵字來指定資料庫的名稱，系統就會在應用程式關閉時從 LocalDB 執行個體中移除資料庫。  
  
- 請在連接字串中指定 LocalDB 執行個體。 例如，您的執行個體名稱是 `myInstance`，連接字串就會包含：  
  
    `server=(localdb)\\myInstance`  
  
 連線到 LocalDB 資料庫時，不允許使用 `User Instance=True`。  
  
如需安裝 LocalDB 的詳細資訊，請參閱 [SQL Server Express LocalDB](/sql/database-engine/configure-windows/sql-server-express-localdb)。
  
## <a name="programmatically-create-a-named-instance"></a>以程式設計方式建立具名執行個體  

 應用程式可以建立具名執行個體，並指定資料庫，如下所示：  
  
- 指定要在 app.config 檔案中建立的 LocalDB 執行個體，如下所示。  執行個體的版本號碼應該與您 LocalDB 安裝的版本號碼相同。  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <configuration>  
      <configSections>  
        <section  
        name="system.data.localdb"  
        type="System.Data.LocalDBConfigurationSection,System.Data,Version=4.0.0.0,Culture=neutral,PublicKeyToken=b77a5c561934e089"/>  
      </configSections>  
      <system.data.localdb>  
        <localdbinstances>  
          <add name="myInstance" version="11.0" />  
        </localdbinstances>  
      </system.data.localdb>  
    </configuration>  
    ```  
  
- 使用 `server` 連接字串關鍵字來指定執行個體的名稱。  在 `server` 連接字串關鍵字中指定的執行個體名稱，必須與 app.config 檔案指定的名稱相符。  
  
- 使用 `AttachDBFilename` 連接字串關鍵字來指定 .MDF 檔案。  
  
## <a name="see-also"></a>請參閱

- [SQL Server 功能和 ADO.NET](sql-server-features-and-adonet.md)
- [ADO.NET 概觀](../ado-net-overview.md) \(部分機器翻譯\)
