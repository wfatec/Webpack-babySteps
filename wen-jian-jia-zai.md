# 文件加载

我们在开发过程中时常会用到一些文件资源，例如图片等。这时就要用到file-loader或url-loader. [file-loader](https://webpack.docschina.org/loaders/file-loader/)可以处理文件对象，并将处理后的文件变成文件内容的MD5 hash，后缀名为源文件的后缀名。[url-loader](https://webpack.docschina.org/loaders/url-loader/) 封装了file-loader，其工作时会分为两类情况：

1. 文件大小小于limit参数，url-loader将会把文件转为DataURL
2. 文件大小大于limit，url-loader会调用file-loader进行处理，参数也会直接传给file-loader。因此我们只需要安装url-loader即可。

这里我们选用url-loader来进行文件处理。

## 安装url-loader

```text
yarn add -D url-loader
```

1. 配置mudule.rules

```javascript
{
    //匹配png jpg gif类型的文件,忽略大小写
    test: /\.(png|jpg|gif)$/i,
    use: [
        {
            /**
             * 可以处理文件对象，并将处理后的文件变成文件内容的MD5 hash，后缀名为源文件的后缀名。
             *          类型          名称           默认值                               描述
             * @param {Number}       limit       'undefined'                  文件小于limit时，以URL方式引入
             * @param {string}      mimetype       extname          是否把其他后缀名的图片文件，统一转为同一种格式的base64编码   
             * @param {string}      fallback     file-loader                文件大于limit时，调用file-loader方式处理
             */
            loader: 'url-loader',
            options: {
                mimetype: 'image/png',
                limit: '8024',
                /**
                 * name表示输出的文件名规则，如果不添加这个参数，输出的就是默认值：文件哈希。
                 * 加上[path]表示输出文件的相对路径与当前文件相对路径相同，
                 * 加上[name].[ext]则表示输出文件的名字和扩展名与当前相同。
                 * 加上[hash]表示加上一个hash码，用于唯一标识打包文件
                 * 加上[path]这个参数后，打包后文件中引用文件的路径也会加上这个相对路径。
                 */
                name: '[name].[hash].[ext]',
                /**
                 *  outputPath表示输出文件路径前缀。图片经过url-loader打包都会打包到指定的输出文件夹下。
                 * 但是我们可以指定图片在输出文件夹下的路径。比如outputPath=img/，
                 * 图片被打包时，就会在输出文件夹下新建（如果没有）一个名为img的文件夹，
                 * 把图片放到里面。
                 */
                outputPath:'img/'
                /**
                 *  publicPath表示打包文件中引用文件的路径前缀，如果你的图片存放在CDN上，
                 * 那么你上线时可以加上这个参数，值为CDN地址，这样就可以让项目上线后的资源引用路径指向CDN了。
                 */
                //publicPath:'output/'
            }
        }
    ]
}
```

接下来在src/img放入一张图片，命名为saber.jpg\(可任意命名\)，在src/App.js中引入

```javascript
import React from "react";
import ReactDOM from "react-dom";
import Saber from './img/saber.jpg';
const App = () => {
  return (
    <div>
      <img src={Saber} alt='亚瑟王' />
    </div>
  );
};
export default App;
ReactDOM.render(<App />, document.getElementById("root"));
```

此时运行`npm run build`，发现控制台报错，大意是缺少一些模块，然后在node\_modules/url-loader和`node_modules`下查找，发现并没有`file-loader`的依赖，因此估计是缺少`file-loader`，那么尝试一下安装`file-loader`:

```text
yarn add -D file-loader
```

之后运行`npm run build`，一切正常，我们的图片也被正常打包到`dist/img`下，命名为`saber.41ba48fec44b8185322755f64d4f3af7.jpg`

