# Chào mừng

Trong hướng dẫn bắt đầu nhanh này, chúng ta sẽ bắt đầu với một dự án khởi đầu đơn giản và sau đó kết thúc bằng cách lập chỉ mục một số dữ liệu thực tế. Đây là cơ sở tuyệt vời để bắt đầu khi phát triển Dự án SubQuery của riêng bạn.

Ở cuối hướng dẫn này, bạn sẽ có một dự án SubQuery đang hoạt động chạy trên nút SubQuery với điểm cuối GraphQL mà có thể truy vấn dữ liệu từ đó.

Nếu chưa có, chúng tôi khuyên bạn nên tự làm quen với [thuật ngữ](../#terminology) được sử dụng trong SubQuery.

**Mục tiêu của hướng dẫn bắt đầu nhanh này là điều chỉnh dự án khởi đầu tiêu chuẩn để bắt đầu lập chỉ mục tất cả các giao dịch từ Polkadot, chỉ mất 10-15 phút**

## Chuẩn bị

### Môi trường phát triển địa phương

- [Node](https://nodejs.org/en/): Cài đặt một phiên bản mới nhất của Node (ví dụ: phiên bản LTS).
- [Docker](https://docker.com/): Hướng dẫn này sẽ yêu cầu sử dụng Docker

### Cài đặt CLI SubQuery

Cài đặt SubQuery CLI tổng thể trên terminal của bạn bằng cách sử dụng NPM:

```shell
# NPM
npm install -g @subql/cli
```

Xin lưu ý rằng chúng tôi **KHÔNG** khuyến khích sử dụng `yarn global<` để cài đặt `@subql/cli` do quản lý phụ thuộc kém có thể dẫn đến lỗi xuống dòng.

Sau đó, bạn có thể chạy help để xem các lệnh có sẵn và cách sử dụng do CLI cung cấp

```shell
subql help
```

## Khởi tạo Dự án khởi đầu SubQuery

Bên trong thư mục mà bạn muốn tạo một dự án SubQuery, chỉ cần chạy lệnh sau để bắt đầu.

```shell
subql init
```

Bạn sẽ được hỏi một số câu hỏi nhất định khi dự án SubQuery được khởi động:

- Name: Tên dự án SubQuery của bạn
- Network: Một chuỗi khối mà dự án SubQuery này sẽ được phát triển để lập chỉ mục, sử dụng các phím mũi tên trên bàn phím của bạn để chọn từ các tùy chọn, đối với hướng dẫn này, chúng tôi sẽ sử dụng *"Polkadot"*
- Template: Chọn mẫu dự án SubQuery sẽ cung cấp điểm khởi đầu để bắt đầu phát triển, chúng tôi gợi ý bạn chọn *"Starter project"*
- Git repository (Tùy chọn): Cung cấp URL Git cho kho lưu trữ dự án SubQuery này (khi được lưu trữ trong SubQuery Explorer)
- RPC endpoint (Bắt buộc): Cung cấp URL HTTPS cho điểm cuối RPC đang chạy, sẽ được sử dụng mặc định cho dự án này. Bạn có thể nhanh chóng truy cập các điểm cuối công khai cho các mạng Polkadot khác nhau hoặc thậm chí tạo nút chuyên dụng riêng của mình bằng cách sử dụng [OnFinality](https://app.onfinality.io) hoặc chỉ sử dụng điểm cuối Polkadot mặc định. Nút RPC này phải là một nút lưu trữ (có trạng thái chuỗi đầy đủ). Đối với hướng dẫn này, chúng tôi sẽ sử dụng giá trị mặc định *"https://polkadot.api.onfinality.io"*
- Authors (Bắt buộc): Nhập chủ sở hữu của dự án SubQuery này tại đây (ví dụ: tên bạn!)
- Description (Tùy chọn): Bạn có thể cung cấp một đoạn giới thiệu ngắn về dự án của mình, mô tả dự án chứa dữ liệu gì và người dùng có thể làm gì với dự án
- Version (Bắt buộc): Nhập số phiên bản tùy chỉnh hoặc sử dụng giá trị mặc định (`1.0.0`)
- License (Bắt buộc): Cung cấp giấy phép phần mềm cho dự án này hoặc chấp nhận mặc định (`Apache-2.0`)

Sau khi quá trình khởi tạo hoàn tất, bạn sẽ thấy một thư mục có tên dự án của bạn đã được tạo bên trong thư mục. Nội dung của thư mục này phải giống với nội dung được liệt kê trong [Cấu trúc thư mục](../create/introduction.md#directory-structure).

Cuối cùng, trong thư mục dự án, chạy lệnh sau để cài đặt các phụ thuộc của dự án mới.

<CodeGroup> <CodeGroupItem title="YARN" active> ```shell cd PROJECT_NAME yarn install ``` </CodeGroupItem>
<CodeGroupItem title="NPM"> ```shell cd PROJECT_NAME npm install ``` </CodeGroupItem> </CodeGroup>

## Thực hiện các thay đổi trên dự án của bạn

Trong gói khởi đầu mà bạn vừa khởi tạo, chúng tôi đã cung cấp cấu hình tiêu chuẩn cho dự án của bạn. Bạn sẽ làm việc chủ yếu trên các tệp sau:

1. Lược đồ GraphQL ở `schema.graphql`
2. Tệp Kê khai dự án ở ` project.yaml `
3. Các chức năng ánh xạ trong thư mục `src/mappings/`

Mục tiêu của hướng dẫn bắt đầu nhanh này là điều chỉnh dự án khởi đầu tiêu chuẩn để bắt đầu lập chỉ mục tất cả các giao dịch từ Polkadot.

### Cập nhật tệp lược đồ GraphQL của bạn

Tệp `schema.graphql` xác định các lược đồ GraphQL khác nhau. Do cách hoạt động của ngôn ngữ truy vấn GraphQL, về cơ bản tệp lược đồ chỉ ra hình dạng dữ liệu của bạn từ SubQuery. Đây là một nơi tuyệt vời để bắt đầu vì nó cho phép bạn xác định trước mục tiêu cuối cùng của mình.

Chúng ta sẽ cập nhật tệp `schema.graphql` để trông như sau

```graphql
type Transfer @entity {
  id: ID! # Trường id là bắt buộc và phải trông như thế này
  amount: BigInt # Số tiền được chuyển
  blockNumber: BigInt # Chiều cao khổi của giao dịch
  from: Account! # Tài khoản chuyển tiền được thực hiện từ
  to: Account! # Tài khoản chuyển tiền được thực hiện cho
}
```

**Quan trọng: Khi bạn thực hiện bất kỳ thay đổi nào đối với tệp lược đồ, hãy đảm bảo rằng bạn tạo lại thư mục types của mình. Thực hiện ngay bây giờ.**

<CodeGroup> <CodeGroupItem title="YARN" active> ```shell yarn codegen ``` </CodeGroupItem>
<CodeGroupItem title="NPM"> ```shell npm run-script codegen ``` </CodeGroupItem> </CodeGroup>

Bạn sẽ tìm thấy các model đã tạo trong `thư mục /src/types/models`. Để biết thêm thông tin về tệp `schema.graphql`, hãy xem tài liệu của chúng tôi trong [Lược đồ Build/GraphQL ](../build/graphql.md)

### Cập nhật tệp kê khai dự án

Tệp Project Manifest (`project.yaml`) có thể được xem là điểm vào dự án của bạn và nó xác định hầu hết các thông tin chi tiết về cách SubQuery sẽ lập chỉ mục và chuyển đổi dữ liệu chuỗi.

Chúng tôi sẽ không thực hiện nhiều thay đổi đối với tệp kê khai vì tệp đã được thiết lập đúng cách, nhưng chúng tôi cần thay đổi trình xử lý của mình. Hãy nhớ rằng chúng tôi đang lên kế hoạch lập chỉ mục tất cả các giao dịch Polkadot, do đó, chúng tôi cần cập nhật phần `datasources` để trông như sau.

```yaml
dataSources:
  - kind: substrate/Runtime
    startBlock: 1
    mapping:
      file: ./dist/index.js
      handlers:
        - handler: handleEvent
          kind: substrate/EventHandler
          filter:
            module: balances
            method: Transfer
```

Điều này có nghĩa là chúng ta sẽ chạy một hàm ánh xạ `handleEvent` mỗi khi có sự kiện `balance.Transfer`.

Để biết thêm thông tin về tệp Project Manifest (`project.yaml`), hãy xem tài liệu của chúng tôi trong [Build/Manifest File](../build/manifest.md)

### Thêm một hàm Ánh xạ

Các hàm ánh xạ xác định cách dữ liệu chuỗi được chuyển đổi thành các thực thể GraphQL được tối ưu hóa mà chúng ta đã xác định trước đó trong tệp `schema.graphql`.

Điều hướng đến hàm ánh xạ mặc định trong thư mục `src/mappings `. Bạn sẽ thấy ba hàm được xuất, `handleBlock`, `handleEvent` và `handleCall`. Bạn có thể xóa cả hai hàm `handleBlock` và `handleCall`, chúng tôi chỉ sử dụng hàm `handleEvent`.

Hàm `handleEvent` nhận dữ liệu sự kiện bất cứ khi nào sự kiện khớp với các bộ lọc mà chúng tôi chỉ định trước đó trong `project.yaml` của chúng tôi. Chúng tôi sẽ cập nhật nó để xử lý tất cả các sự kiện `balance.Transfer` và lưu chúng vào các thực thể GraphQL mà chúng tôi đã tạo trước đó.

Bạn có thể cập nhật hàm `handleEvent` như sau (lưu ý các import bổ sung):

```ts
import { SubstrateEvent } from "@subql/types";
import { Transfer } from "../types";
import { Balance } from "@polkadot/types/interfaces";

export async function handleTransfer(event: SubstrateEvent): Promise<void> {
    // Get data from the event
    // The balances.transfer event has the following payload \[from, to, value\]
    // logger.info(JSON.stringify(event));
    const from = event.event.data[0];
    const to = event.event.data[1];
    const amount = event.event.data[2];

    // Create the new transfer entity
    const transfer = new Transfer(
        `${event.block.block.header.number.toNumber()}-${event.idx}`,
    );
    transfer.blockNumber = event.block.block.header.number.toBigInt();
    transfer.from = from.toString();
    transfer.to = to.toString();
    transfer.amount = (amount as Balance).toBigInt();
    await transfer.save();
}
```

Hàm này đang nhận SubstrateEvent bao gồm dữ liệu truyền tải trên trọng tải. Chúng tôi trích xuất dữ liệu này và sau đó khởi tạo thực thể `Transfer` mới mà chúng tôi đã xác định trước đó trong tệp `schema.graphql`. Chúng tôi thêm thông tin bổ sung và sau đó sử dụng hàm `.save()` để lưu thực thể mới (SubQuery sẽ tự động lưu nó vào cơ sở dữ liệu).

Để biết thêm thông tin về các hàm ánh xạ, hãy xem tài liệu của chúng tôi trong [Build/Mappings](../build/mapping.md)

### Xây dựng dự án

Để chạy Dự án SubQuery mới của bạn trước tiên chúng tôi cần xây dựng công việc của mình. Chạy lệnh xây dựng từ thư mục gốc của dự án.

<CodeGroup> <CodeGroupItem title="YARN" active> ```shell yarn build ``` </CodeGroupItem> <CodeGroupItem title="NPM"> ```shell npm run-script build ``` </CodeGroupItem> </CodeGroup>

**Quan trọng: Bất cứ khi nào bạn thực hiện các thay đổi đối với các hàm ánh xạ của mình, bạn sẽ cần phải xây dựng lại dự án của mình**

## Chạy và truy vấn dự án của bạn

### Chạy Dự án của bạn với Docker

Bất cứ khi nào bạn tạo một Dự án SubQuery mới, bạn nên chạy nó cục bộ trên máy tính của mình để kiểm tra nó trước. Cách dễ nhất để làm điều này là sử dụng Docker.

Tất cả cấu hình kiểm soát cách chạy node SubQuery được định nghĩa trong tệp ` docker-comp.yml`. Đối với một dự án mới vừa được khởi tạo, bạn sẽ không cần phải thay đổi bất kỳ điều gì nhưng có thể đọc thêm về tệp và cài đặt trong [phần Chạy dự án](../run_publish/run.md) của chúng tôi

Trong thư mục dự án chạy lệnh sau:

<CodeGroup> <CodeGroupItem title="YARN" active> ```shell yarn start:docker ``` </CodeGroupItem> <CodeGroupItem title="NPM"> ```shell npm run-script start:docker ``` </CodeGroupItem> </CodeGroup>

Có thể mất một chút thời gian để tải xuống các gói cần thiết ([`@subql/node`](https://www.npmjs.com/package/@subql/node), [`@subql/query`](https://www.npmjs.com/package/@subql/query), và Postgres) cho lần đầu tiên, nhưng bạn sẽ sớm thấy một node SubQuery đang chạy. Hãy kiên nhẫn ở bước này.

### Truy vấn dự án của bạn

Mở trình duyệt của bạn và truy cập [ http://localhost:3000](http://localhost:3000).

Bạn sẽ thấy một sân chơi GraphQL đang hiển thị trong explorer và các lược đồ đã sẵn sàng để truy vấn. Ở trên cùng bên phải của sân chơi, bạn sẽ tìm thấy nút _Tài liệu_ sẽ mở bản vẽ tài liệu. Tài liệu này được tạo tự động và giúp bạn tìm thấy những thực thể và phương pháp nào bạn có thể truy vấn.

Đối với dự án khởi đầu SubQuery mới, bạn có thể thử truy vấn như sau để biết cách hoạt động của nó hoặc [tìm hiểu thêm về ngôn ngữ Truy vấn GraphQL](../run_publish/graphql.md).

```graphql
{
  query {
    transfers(
      first: 10,
      orderBy: AMOUNT_DESC
    ) {
      nodes {
        id
        amount
        blockNumber
        from
        to
      }
    }
  }
}
```

### Xuất bản Dự Án SubQuery của bạn

SubQuery cung cấp dịch vụ quản lý miễn phí nơi bạn có thể triển khai dự án mới của mình. Bạn có thể triển khai nó trên [SubQuery Projects](https://project.subquery.network) và truy vấn nó bằng cách sử dụng [Explorer](https://explorer.subquery.network) của chúng tôi.

[Đọc hướng dẫn để xuất bản dự án mới của bạn lên SubQuery Projects](../run_publish/publish.md)

## Bước tiếp theo

Xin chúc mừng, bạn hiện có một dự án SubQuery đang chạy cục bộ chấp nhận các yêu cầu API GraphQL để chuyển dữ liệu.

Bây giờ bạn đã có cái nhìn sâu sắc về cách xây dựng một dự án SubQuery cơ bản, câu hỏi đặt ra là bắt đầu từ đâu? Nếu bạn cảm thấy tự tin, bạn có thể bắt đầu tìm hiểu thêm về ba tệp chính. Tệp kê khai, lược đồ GraphQL và tệp ánh xạ trong phần [Phần Xây dựng của các tài liệu này](../build/introduction.md).

Nếu không, hãy tiếp tục đến [Phần Học viện](../academy/academy.md) của chúng tôi, nơi có các hội thảo, hướng dẫn và dự án mẫu chuyên sâu hơn. Ở đó, chúng ta sẽ xem xét các sửa đổi nâng cao hơn và chúng ta sẽ đi sâu hơn vào việc chạy các dự án SubQuery bằng cách chạy các dự án nguồn mở và sẵn có.

Cuối cùng, nếu bạn đang tìm kiếm các cách khác để chạy và xuất bản dự án của mình, [Chạy & Phần xuất bản](../run_publish/run.md) cung cấp thông tin chi tiết về tất cả các cách để chạy dự án SubQuery của bạn và các tính năng tổng hợp GraphQL và tính năng theo dõi.
