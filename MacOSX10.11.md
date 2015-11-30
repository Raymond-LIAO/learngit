
希望有时间完成这个project：整理下来自己的mac os 的 setting方案 希望可以便利或者方便和自己一样的喜欢mac os x的科研狗。顺便可以复习一下MD的使用。

# Mac OS X 10.11 El Capitan 系统个人化设置参考

可以使用[Cask](http://caskroom.io/)来安装绝大部分不在MacAppStore的软件，但是之前需要首先安装[homebrew](http://brew.sh/)：

	ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

之后只需要简单的一句：
	
	brew install caskroom/cask/brew-cask


## Apps from MacAppStore 
* [WeatherWall](https://itunes.apple.com/us/app/weather-wall/id581893121?mt=12) 漂亮的屏幕软件，付费
* [The Unarchiver](https://itunes.apple.com/en/app/the-unarchiver/id425424353?mt=12) 压缩和解压缩，免费
* [Alternote](https://itunes.apple.com/app/id974971992?mt=12
) Evernote的客户端，简介好看，集中精力
* [Pocket](https://itunes.apple.com/us/app/pocket/id568494494?mt=12) 知名RIL客户端，帮助我整理收集
* [CleanMyDrive](https://itunes.apple.com/us/app/cleanmydrive-external-drives/id523620159?mt=12) Macpaw家的免费清理软件
* [1Checker](https://itunes.apple.com/us/app/1checker/id766176336?mt=12) 免费的英语拼写检查，论文神器之一
* [Notability](https://itunes.apple.com/us/app/notability/id736189492?mt=12) Pad上最常用的笔记软件的Mac版本

## Other Apps（都可以通过brew cask install ）
* [MplayerX](http://mplayerx.org/download.html#sthash.bF26vnbD.Zgsb1Shh.dpbs) 全能播放王，免费
* [CleanMyMac3](http://macpaw.com/landings/land221?campaign=search_text_cmm3_brand_rw&utm_source=&utm_medium=&utm_term=&utm_content=&utm_campaign=&gclid=Cj0KEQiA-NqyBRC905irsrLr-LUBEiQAWJFYTuPKx1v1DbJeGAnDRupaPIUZNBx1yZ11ItOGOlThre4aArEN8P8HAQ&siteID=&CJPID=&mpaid=) Mac上面最好的清理工具，巨贵无比
* [Alfred2](https://www.alfredapp.com/) 最知名的Mac平台启动器之一
* [Bartender2](https://www.macbartender.com/) 状态栏管理，让你的电脑更清爽
* [TrimEnabler](https://www.cindori.org/software/trimenabler/) SSD维护
* [f.lux](https://justgetflux.com/news/pages/mac/) 保护眼睛
* [iStatMenus](https://bjango.com/mac/istatmenus/) 时刻监控你的Mac
* [Dropbox](https://www.dropbox.com/) 不用介绍了吧
* [GoogleChrome](https://www.google.com/chrome/browser/desktop/index.html) 不用介绍了吧 虽然依赖免费服务不是一件好事，但是有很多Google的东西已经让我欲罢不能了
* [Jumpcut](http://jumpcut.sourceforge.net/) 免费开源的剪切板工具
* [Steam](http://store.steampowered.com/) 总还是需要休闲一下滴

## Useful for working
* [Matlab](http://fr.mathworks.com/products/matlab/?refresh=true) 最常用
* [Omnifocus](https://www.omnigroup.com/omnifocus) 知名的GTD工具，学习成本高，效果好
* [RescueTime](https://www.rescuetime.com) 节约时间，提高效率
* [iTerm2](https://www.iterm2.com/) 原生Terminal替换产品
* [skim](http://skim-app.sourceforge.net/) 原生Preview替换产品
* [SublimeText](https://www.sublimetext.com/) 最喜欢，最常用的编辑器
* [MacTex](https://tug.org/mactex/) Mac上的Tex


## OS X Preferences
```
%将Library文件夹设置为显示
chflags nohidden ~/Library

%将截屏保存到桌面的Screenshots子文件夹
mkdir ~/Desktop/Screenshots
defaults write com.apple.screencapture location ~/Desktop/Screenshots
```

## Settings for Terminal(iTerm2)
## Settings for SublimeText3
### Add Sublime Text CLI

	sudo rm /usr/local/bin/subl
	sudo ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl /usr/local/bin/subl

当然如果发现提示不存在文件夹时，需要先

	touch /usr/local/bin

### Install Package Control
打开View > Show Console menu：

	import urllib.request,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404' + 'e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)

### Plug-ins
* [SideBarEnhancements](https://packagecontrol.io/packages/SideBarEnhancements)
* [BracketHighlighter](https://packagecontrol.io/packages/BracketHighlighter)
* [Alignment](https://packagecontrol.io/packages/Alignment)
* [Git](https://packagecontrol.io/packages/Git)
* [Markdown Preview)](https://packagecontrol.io/packages/Markdown%20Preview)
* [Terminal](https://packagecontrol.io/packages/Terminal)
* [LaTeXTools](https://packagecontrol.io/packages/LaTeXTools)

### Themes
* [Theme Soda](https://packagecontrol.io/packages/Theme%20-%20Soda)
	* 此处需要一句：`"theme": "Soda Dark 3.sublime-theme"`

## Settings for Mail

##Git
###Git Setups

	ssh-keygen -t rsa -C "raymondedge928@gmail.com"

	% Copy ssh key to github.com
	subl ~/.ssh/id_rsa.pub

	% Test connection
	ssh -T git@github.com

	% Set git config values
	git config --global user.name "Raymond LIAO"
	git config --global user.email "raymondedge928@gmail.com"
	git config --global github.user Raymond-LIAO
	git config --global github.token 8e900959a58d553e8d8fffcfd1a242232d743203

	git config --global core.editor "subl -w"
	git config --global color.ui true

##参考资料
* [GitHub上kevinelliott的设置](https://gist.github.com/kevinelliott/e12aa642a8388baf2499)
* [关于sublimeText的CommandLineTool在MacOS10.11的解决方案](http://stackoverflow.com/questions/32915464/sublime-symlink-disappeared-after-upgrading-to-el-capitan)
* [廖雪峰的Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013752340242354807e192f02a44359908df8a5643103a000)



