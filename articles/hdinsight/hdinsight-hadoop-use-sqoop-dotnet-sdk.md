<properties
	pageTitle="在 HDInsight 上使用 Hadoop Sqoop | Microsoft Azure"
	description="了解如何在 Hadoop 叢集與 Azure SQL Database 之間使用 HDInsight .NET SDK 執行 Sqoop 匯入和匯出。"
	editor="cgronlun"
	manager="paulettm"
	services="hdinsight"
	documentationCenter=""
	tags="azure-portal"
	authors="mumian"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
   ms.date="06/28/2016"
	ms.author="jgao"/>

#在 HDInsight 中使用 .NET SDK for Hadoop 執行 Sqoop 工作

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

了解如何在 HDInsight 上使用 HDInsight .NET SDK for Hadoop 執行 Sqoop 工作，以進行 HDInsight 叢集與 Azure SQL Database 或 SQL Server Database 之間的匯入和匯出作業。

> [AZURE.NOTE] 本文中的步驟可以與 Windows 架構或 Linux 架構的 HDInsight 叢集搭配使用。不過，這些步驟只能從 Windows 用戶端運作。您可使用本文頂端的索引標籤選取器，以選擇其他方法。

###必要條件

開始進行本教學課程之前，您必須具備下列條件：

- **HDInsight 中的 Hadoop 叢集**。請參閱[建立叢集與 SQL Database](hdinsight-use-sqoop.md#create-cluster-and-sql-database)。

## 使用 .NET SDK 執行 Sqoop

HDInsight .NET SDK 提供 .NET 用戶端程式庫，讓您輕鬆地從 .NET 使用 HDInsight 叢集。在本節中，您會建立 C# 主控台應用程式，將 hivesampletable 匯出至您稍早在本教學課程中建立的 SQL Database 資料表。

**提交 Sqoop 工作**

1. 在 Visual Studio 建立 C# 主控台應用程式。
2. 從 Visual Studio Package Manager Console 執行下列 Nuget 命令以匯入套件。

        Install-Package Microsoft.Azure.Management.HDInsight.Job
        
3. 在 Program.cs 檔案中使用下列程式碼：

        using System.Collections.Generic;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
        
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
        
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
        
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
        
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
        
                    SubmitSqoopJob();
        
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
        
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
        
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
        
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
        
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
        
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
        
4. 按 **F5** 鍵執行程式。

##限制

* 大量匯出 - 使用 Linux 型 HDInsight，用來將資料匯出至 Microsoft SQL Server 或 Azure SQL Database 的 Sqoop 連接器目前不支援大量插入。

* 批次處理 - 使用 Linux 型 HDInsight，執行插入時若使用 `-batch` 參數，Sqoop 將會執行多個插入，而不是批次處理插入作業。

##後續步驟

現在，您已了解如何使用 Sqoop。若要深入了解，請參閱：

- [搭配 HDInsight 使用 Oozie](hdinsight-use-oozie.md)：在 Oozie 工作流程中使用 Sqoop 動作。
- [使用 HDInsight 分析航班延誤資料](hdinsight-analyze-flight-delay-data.md)：使用 Hive 分析航班誤點資料，然後使用 Sqoop 將資料匯出至 Azure SQL Database。
- [將資料上傳至 HDInsight](hdinsight-upload-data.md)：尋找可將資料上傳至 HDInsight/Azure Blob 儲存體的其他方法。

<!---HONumber=AcomDC_0720_2016-->