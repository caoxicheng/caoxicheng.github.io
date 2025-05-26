---
title: Angular-监听文件上传进度
date: '2020-03-20 13:51'
tags: []
categories:
  - 技术
  - Angular
excerpt: >-
  获取文件 <input type="file" (input)="inputFile(file.files[0])" #file> 服务
  uploadFile(file): Observable<any> {     const req = new HttpRequest('POST',
  '/upl...
description: Angular-监听文件上传进度 - 详细介绍与实践经验分享
keywords:
  - Angular
  - 监听文件上传进度
  - 技术
cover: ''
---

## 获取文件

```
<input type="file" (input)="inputFile(file.files[0])" #file>
```

## 服务

```
uploadFile(file): Observable<any> {
    const req = new HttpRequest('POST', '/upload/file', file, {
      reportProgress: true
    });

    return this.http.request(req).pipe(
      map(event => this.getEventMessage(event as HttpEvent<any>, file)), //这里ts编译时出了点类型错误，使用强制声明
      tap(message => console.log(message)),
      last(), // return last (completed) message to caller
      catchError(this.handleError(file))
    );

    //也可以不用HttpRequest, 需要额外的ovserve: 'events'
    return this.http.request('POST', 'URL', { reprtProgress: true, ovserve: 'events' , body: file}).pipe(同上)
}


private getEventMessage(event: HttpEvent<any>, file: File) {
  switch (event.type) {
    case HttpEventType.Sent:
      return `Uploading file "${file.name}" of size ${file.size}.`;

    case HttpEventType.UploadProgress:
      // Compute and show the % done:
      const percentDone = Math.round(100 * event.loaded / event.total);
      return `File "${file.name}" is ${percentDone}% uploaded.`;

    case HttpEventType.Response:
      return `File "${file.name}" was completely uploaded!`;

    default:
      return `File "${file.name}" surprising upload event: ${event.type}.`;
  }
}
```

## 调用

```
this.uploadFile(file).subscribe( res => {}, error? => {}, compplete? => {})
```

---

说点别的

RXJS操作符还有点不太熟悉，先会用，再考虑深入！

下一篇文章写在下进度吧,应该同理，在options里面增加参数，并监听progress
