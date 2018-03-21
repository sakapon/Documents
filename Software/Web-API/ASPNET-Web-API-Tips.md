# ASP.NET Web API の Tips 集
ASP.NET Web API を利用する際の注意点や備忘録です。ほぼ箇条書きです。  
基本的な説明は省略しています。

## ルーティング
WebApiConfig.cs にルーティングの設定が記述されています。  
既定のテンプレートは "api/{controller}/{id}" となっており、REST スタイルを想定したものとなっています。  
ただし、ASP.NET MVC と同様に {action} も利用可能であり、RPC スタイルの API も構成できます。

- [RoutePrefix], [Route], [ActionName] などの属性を利用することで柔軟に構成できる
- [Route] を複数設定できる

## HTTP メソッド
- 主に REST スタイルの場合、Get、Post などのメソッド名で解決される (CoC)。この場合、[HttpGet] などの属性を指定する必要はない
- 主に RPC スタイルで任意のアクション名を利用するには、[HttpGet] などの属性を指定する

## 引数

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net;
using System.Net.Http;
using System.Web.Http;

namespace SampleWebApi.Controllers
{
    [RoutePrefix("api/Random")]
    [Route("{action}")]
    public class RandomController : ApiController
    {
        static readonly Random random = new Random();

        [HttpGet]
        [Route("Echo/{i:int?}")]
        public int Echo(int i = 123) => i;

        [HttpGet]
        [Route("NewDoubles/{count:int:range(0,64)}")]
        public double[] NewDoubles(int count)
        {
            return Enumerable.Range(0, count)
                .Select(i => random.NextDouble())
                .ToArray();
        }

        [HttpGet]
        [Route(@"NewDateTime/{date:datetime:regex(\d{4}-\d{2}-\d{2})}")]
        [Route(@"NewDateTime/{*date:datetime:regex(\d{4}/\d{2}/\d{2})}")]
        public DateTime NewDateTime(DateTime date)
        {
            return date + TimeSpan.FromHours(24 * random.NextDouble());
        }

        [HttpGet]
        public Guid NewUuid()
        {
            return Guid.NewGuid();
        }
    }
}
```

### 作成したサンプル
- [AspNetWebApiSample (GitHub)](https://github.com/sakapon/Samples-2018/tree/master/AspNetWebApiSample)

### バージョン情報
- .NET Framework 4.5
- ASP.NET Web API 5.2.3

### 参照
- [ASP.NET Web API](https://docs.microsoft.com/en-us/aspnet/web-api/)
