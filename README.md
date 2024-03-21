<main class="wp-block-group is-layout-flow wp-block-group-is-layout-flow" id="wp--skip-link--target">
<div class="wp-block-group is-layout-constrained wp-block-group-is-layout-constrained"><h1 style="" class="alignwide wp-block-post-title">mpy固件编译成功</h1>
![image](1.png)
<hr class="wp-block-separator has-css-opacity alignwide is-style-wide">
</div>
<div style="height: 32px;" aria-hidden="true" class="wp-block-spacer"></div>
<h5 class="wp-block-heading">前情提要：ubuntu 20.04 ltsc，vps（解决网络问题)</h5>
<p><strong>具体流程</strong></p>
<p>更新源列表</p>
<pre class="wp-block-code"><code>sudo apt-get update
sudo apt-get upgrade  #Optional</code></pre>
<hr class="wp-block-separator has-alpha-channel-opacity">
<p><strong>python配置</strong>        将默认python版本改为python3</p>
<div class="wp-block-group is-nowrap is-layout-flex wp-container-core-group-layout-5 wp-block-group-is-layout-flex">
<p><br>查看当前系统默认python版本</p>
<pre class="wp-block-code"><code>python --version</code></pre>
</div>
<div class="wp-block-group is-nowrap is-layout-flex wp-container-core-group-layout-6 wp-block-group-is-layout-flex">
<p>查看python3</p>
<pre class="wp-block-code"><code>whereis python3</code></pre>
</div>
<div class="wp-block-group is-nowrap is-layout-flex wp-container-core-group-layout-7 wp-block-group-is-layout-flex">
<p>删除原有python2的软连接</p>
<pre class="wp-block-code"><code>sudo rm /usr/bin/python</code></pre>
</div>
<p>新建python3的软连接</p>
<pre class="wp-block-code"><code>sudo ln -s /usr/bin/python3.8 /usr/bin/python</code></pre>
<div class="wp-block-group is-nowrap is-layout-flex wp-container-core-group-layout-8 wp-block-group-is-layout-flex">
<p>安装pip</p>
<pre class="wp-block-code"><code>apt install python3-pip
</code></pre>
</div>
<div class="wp-block-group is-nowrap is-layout-flex wp-container-core-group-layout-9 wp-block-group-is-layout-flex">
<p>安装virtualenv！！</p>
<pre class="wp-block-code"><code>pip install virtualenv</code></pre>
</div>
<hr class="wp-block-separator has-alpha-channel-opacity">
<div class="wp-block-group is-layout-constrained wp-block-group-is-layout-constrained">
<p><strong>安装必备软件</strong></p>
<div class="wp-block-group is-nowrap is-layout-flex wp-container-core-group-layout-10 wp-block-group-is-layout-flex">
<p>Git：</p>
<pre class="wp-block-code"><code>sudo apt-get install git</code></pre>
</div>
<p>资源库中，有一个名为 build-essential 的包，它包含 GCC 和其他各种编译器，如 g++ 和 make来安装：</p>
<pre class="wp-block-code"><code>sudo apt update
sudo apt install build-essential</code></pre>
<div class="wp-block-group is-nowrap is-layout-flex wp-container-core-group-layout-11 wp-block-group-is-layout-flex">
<p>cmake:</p>
<pre class="wp-block-code"><code>apt-get install cmake</code></pre>
</div>
<h6 class="wp-block-heading">基于arm内核编译的工具链：(可选的）</h6>
<pre class="wp-block-code"><code>sudo apt-get install gcc-arm-none-eabi</code></pre>
</div>
<div class="wp-block-group is-layout-constrained wp-block-group-is-layout-constrained">
<hr class="wp-block-separator has-alpha-channel-opacity">
<p><strong>Git克隆</strong> #网络环境<br>esp-idf克隆：注意 esp-idf版本！！</p>
<pre class="wp-block-code"><code>git clone -b v5.0.2 --recursive https://github.com/espressif/esp-idf.git</code></pre>
<p>若esp-idf已经存在，请更新模块</p>
<pre class="wp-block-code"><code>cd esp-idf
git checkout v5.0.2
git submodule update --init --recursive</code></pre>
<h6 class="wp-block-heading">micropython源码克隆： </h6>
<pre class="wp-block-code"><code>git clone -b v1.21.0 --recursive https://github.com/micropython/micropython.git</code></pre>
</div>
<hr class="wp-block-separator has-alpha-channel-opacity">
<div class="wp-block-group is-layout-constrained wp-block-group-is-layout-constrained">
<p><strong>micropython固件编译的安装</strong></p>
<h6 class="wp-block-heading">ESP-IDF 配置好编译环境</h6>
<pre class="wp-block-code"><code>cd esp-idf
./install.sh
source export.sh</code></pre>
<h6 class="wp-block-heading">编译 MicroPython</h6>
<p class="has-small-font-size">必须先构建 MicroPython 交叉编译器，这将用于 预编译（冻结）内置 Python 代码。这个交叉编译器是构建的，并且 使用以下命令在主机上运行：</p>
<pre class="wp-block-code"><code>make -C mpy-cross</code></pre>
<pre class="wp-block-code"><code>cd ports/esp32
make submodules
make</code></pre>
<p>生成的固件为：<code>firmware.bin   ..build-ESP32_GENERIC/</code></p>
<p>如果是编译esp32c3，在make后缀加型号：</p>
<pre class="wp-block-code"><code>make BOARD=GENERIC_C3</code></pre>
</div>
<hr class="wp-block-separator has-alpha-channel-opacity">
<h2 class="wp-block-heading" id="user-content-defining-a-custom-esp32-board"><a href="https://github.com/micropython/micropython/tree/master/ports/esp32#defining-a-custom-esp32-board">定义自定义 ESP32 开发板</a></h2>
<p>默认的 ESP-IDF 配置设置由目录中的 board 定义提供。对于自定义配置 您可以定义自己的 Board 目录。通过以下方式开始新的电路板配置 复制现有的（如）并修改它以适合您的开发板。<code>ESP32_GENERIC</code><code>boards/ESP32_GENERIC</code><code>ESP32_GENERIC</code></p>
<p>特定于 MicroPython 的配置值在特定于主板的文件中定义，该文件包含在 中。附加 设置被放入 ，包括配置 ESP-IDF 设置的文件列表。一些标准文件是 在目录中提供，例如 .您可以 此外，在 Board 目录中定义自定义的。<code>mpconfigboard.h </code><code>mpconfigport.h </code><code>mpconfigboard.cmake </code><code>sdkconfig  </code><code>boards/</code><code>boards/sdkconfig.ble</code></p>
<hr class="wp-block-separator has-alpha-channel-opacity">
<div class="wp-block-group is-vertical is-layout-flex wp-container-core-group-layout-15 wp-block-group-is-layout-flex">
<p class="has-large-font-size">如何在micropython固件中加入自己的py库</p>
<p>mpy的源码结构在不断更新<br>目前只需要找到对应的port文件夹下的modules就可以了，往里面直接扔py文件就会自动被编译到固件了<br>原理也不难，当前的manifest.py自动freeze了modules这个文件夹<br>比如esp32<br>就是ports\esp32\moduels</p>
</div>
<hr class="wp-block-separator has-alpha-channel-opacity">
<p class="has-large-font-size">Raspberry RP2040</p>
<p class="has-small-font-size">必须先构建 MicroPython 交叉编译器，这将用于 预编译（冻结）内置 Python 代码。这个交叉编译器是构建的，并且 使用以下命令在主机上运行：</p>
<pre class="wp-block-code"><code>make -C mpy-cross</code></pre>
<p class="has-small-font-size">此命令应从此存储库的根目录执行。 下面的所有其他命令都应该从 ports/rp2/ 目录执行。</p>
<p class="has-small-font-size">RP2 固件的构建完全使用 CMake 完成，尽管简单 为了方便起见，还提供了 Makefile。要构建固件，请运行（从 此目录）：</p>
<pre class="wp-block-code"><code>make submodules
make clean
make</code></pre>
<p class="has-small-font-size">如果你使用的是Rasoberry Pi Pico以外的其他板，那么你应该 将开发板名称传递给构建;例如，对于Raspberry Pi Pico W：</p>
<pre class="wp-block-code"><code>make BOARD=RPI_PICO_W submodules
make BOARD=RPI_PICO_W clean
make BOARD=RPI_PICO_W</code></pre>
<hr class="wp-block-separator has-alpha-channel-opacity">
<div class="wp-block-group is-vertical is-layout-flex wp-container-core-group-layout-16 wp-block-group-is-layout-flex">
<p>参考资料：</p>
<p>[1]github.<a href="https://github.com/micropython/micropython/tree/master/ports/esp32">micropython esp32</a></p>
<p>[2]github.<a href="https://github.com/micropython/micropython/tree/master/ports/rp2">micropython rp2</a></p>
<p>[3]csdn,<a href="https://blog.csdn.net/jd3096/article/details/120641783">在micropython固件中加入自己的py库</a></p>
<p>[4]csdn.<a href="https://zhuanlan.zhihu.com/p/153124468" data-type="URL" data-id="https://zhuanlan.zhihu.com/p/153124468" target="_blank" rel="noreferrer noopener">wsl代理</a></p>
</div>
</div>
<div style="height: 32px;" aria-hidden="true" class="wp-block-spacer"></div>
<div class="wp-block-group is-layout-constrained wp-block-group-is-layout-constrained">
<div class="wp-block-group is-layout-flex wp-block-group-is-layout-flex"><div style="font-style: italic; font-weight: 400;" class="wp-block-post-date has-small-font-size"><time datetime="2023-01-17T00:19:48+08:00">1月 17, 2023</time></div>
<div class="wp-block-post-author has-small-font-size"><div class="wp-block-post-author__content"><p class="wp-block-post-author__name">louis16s</p></div></div>
<div class="taxonomy-category wp-block-post-terms has-small-font-size"><a href="http://192.168.5.111/index.php/category/test/" rel="tag">嵌入式开发</a></div>
</div>
<div style="height: 32px;" aria-hidden="true" class="wp-block-spacer"></div>
</div>
</main>
<div class="wp-block-group is-nowrap is-layout-flex wp-container-core-group-layout-20 wp-block-group-is-layout-flex">
<div class="wp-block-buttons is-layout-flex wp-block-buttons-is-layout-flex">
<div class="wp-block-button"><a class="wp-block-button__link wp-element-button" href="http://192.168.5.111/wp-admin" target="_blank" rel="noreferrer noopener">登录</a></div>
</div>
<div class="wp-block-buttons is-layout-flex wp-block-buttons-is-layout-flex">
<div class="wp-block-button"><a class="wp-block-button__link wp-element-button">RSS</a></div>
</div>
</div>
</div>
</div>
</div>
</footer></div>
