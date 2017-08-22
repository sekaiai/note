```
"php": "C:/wamp/bin/php/php7.0.0/php.exe",
"nodejs": "C:/Program Files/nodejs/node.exe",
```

## Package Control
[https://packagecontrol.io/installation](https://packagecontrol.io/installation)  
The console is accessed via the `ctrl+` shortcut or the `View > Show Console` menu
```
# install
import urllib.request,os,hashlib; h = 'df21e130d211cfc94d9b0905775a7c0f' + '1e3d39e33b79698005270310898eea76'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

## MarkdownEditing
[https://github.com/SublimeText-Markdown/MarkdownEditing](https://github.com/SublimeText-Markdown/MarkdownEditing)  
`markdown` 在 `sublime` 里面站着比较舒服 

## OmniMarkupPreviewer
[https://github.com/timonwong/OmniMarkupPreviewer](https://github.com/timonwong/OmniMarkupPreviewer)  
用来预览 `markdown` 编辑的效果，同样支持渲染代码高亮的样式。

__*shortcut*__

* `Ctrl` + `Alt` + `O`: Preview Markup in Browser.
* `Ctrl` + `Alt` + `X`: Export Markup as HTML.
* `Ctrl` + `Alt` + `C`: Copy Markup as HTML.


## HTML-CSS-JS Prettify
格式化代码，需要设置 `node path`
Documentation: [https://github.com/victorporof/Sublime-HTMLPrettify](https://github.com/victorporof/Sublime-HTMLPrettify/blob/master/README.md)  

## sublimeCodeIntel
按住`alt+鼠标左键`，可以实现自定义函数之间的跳转，方便查找和修改函数内容和读写代码！
需要设置 `php` 路径  
Git - [https://github.com/SublimeCodeIntel/SublimeCodeIntel](https://github.com/SublimeCodeIntel/SublimeCodeIntel/blob/master/README.rst)
```
# settinngs user
{
    "codeintel_language_settings": {
        "PHP": {
            "php": "C:/wamp/bin/php/php7.0.0/php.exe",
        }
    }
}
```

## Emmet
这个化成灰都能知道你是干啥的。  
Emmet Documentation: [https://docs.emmet.io/cheat-sheet/](https://docs.emmet.io/cheat-sheet/)

## phpfmt
`php` 格式化
```
# setting user
{
    "version": 4,
    "php_bin": "C:/wamp/bin/php/php7.0.0/php.exe",
    "format_on_save": true,
    "option": "value"
}
```

## javascript completions
`javascript` 代码自动补全

## DocBlockr
代码注释插件

## css format
`css` 代码格式化&压缩

