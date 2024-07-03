import { rcp } from "@kit.RemoteCommunicationKit";

@CustomDialog
struct CustomDialogExample {
  @State message: string = "标题";
  @State src: string = '';
  controller: CustomDialogController = new CustomDialogController({
    builder: CustomDialogExample({}),
  });

  build() {
    Column() {
      Text(this.message)
        .fontSize(20)
        .fontWeight('bold')
        .width('100% - 20')
        .margin({ top: 20});
      Image(this.src)
        .width('100%')
        .height('auto')
        .margin({ top: 10 });
    }
  }
}

@Entry
@Component
struct Index {
  @State message: string = '欢迎使用EasyFetch';
  @State reg_1: RegExp = new RegExp('(?<=<b> )(.*?)(?=</b>)');
  @State reg_2: RegExp = new RegExp('(?<=SRC=")(.*?)(?=")');
  @State host: string = 'https://apod.nasa.gov/';
  @State url: string = 'https://apod.nasa.gov/apod/astropix.html';
  build() {
    Row() {
      Column() {
        Text(this.message)
          .id('Title')
          .fontSize(24)
          .fontWeight(FontWeight.Bold)
          .alignRules({
            middle: { anchor: '__container__', align: HorizontalAlign.Center }
          })
          .margin({
            top: 32
          });
        Button() {
          Text('Fetch')
            .fontSize(16)
            .fontColor('#fff');
        }
        .type(ButtonType.Capsule)
        .alignRules({
          middle: { anchor: '__container__', align: HorizontalAlign.Center }
        })
        .margin({
          top: 24
        })
        .backgroundColor('#000')
        .width('40%')
        .height('5%')
        .onClick(() => {
          const request = new rcp.Request(this.url, 'GET');
          const session = rcp.createSession();
          session.fetch(request).then((rep: rcp.Response) => {
            const soup = rep.toString();
            console.log(soup);
            const titleMatch = soup!.match(this.reg_1);
            const imageMatch = soup!.match(this.reg_2);

            if (titleMatch && imageMatch) {
              const title = titleMatch[0];
              const body = this.host + imageMatch[0];
              this.showDialog(title, body);
            } else {
              console.error("Unable to find title or image in the response.");
            }
            session.close();
          }).catch(() => {
            session.close();
          });
        });
      }
      .width('100%');
    }
    .height('100%');
  }

  showDialog(title: string, imageUrl: string) {
    new CustomDialogController({
      builder: CustomDialogExample({
        message: title,
        src: imageUrl,
      }),
    }).open();
  }
}
