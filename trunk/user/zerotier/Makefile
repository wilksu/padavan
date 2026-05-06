# 定义名称和 URL
SRC_NAME = zerotier-one
# 使用你提供的 mipsel 压缩包链接
SRC_URL = https://github.com/lmq8267/ZeroTierOne/releases/download/1.16.0/zerotier.tar.gz

THISDIR = $(shell pwd)

# all 目标：包含下载和解压，确保构建镜像前文件已就绪
all: download_test extract_test
	@echo "ZeroTier 预编译包处理完成。"

# 下载：如果不存在压缩包，则从 GitHub 下载
download_test:
	( if [ ! -f zerotier.tar.gz ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O zerotier.tar.gz $(SRC_URL); \
	fi )

# 解压：直接解压出二进制和软链接
extract_test:
	if [ ! -f $(SRC_NAME) ] ; then \
		tar -xzvf zerotier.tar.gz ; \
		chmod +x $(SRC_NAME) ; \
	fi ;

# 清理：删除下载的文件，保持源码树整洁
clean:
	rm -rf $(SRC_NAME) zerotier.tar.gz zerotier-cli zerotier-idtool

# 核心：将解压出的文件放入固件镜像
romfs:
	# 1. 安装主二进制文件
	$(ROMFSINST) $(THISDIR)/$(SRC_NAME) /usr/bin/zerotier-one
	
	# 2. 安装启动脚本（确保你已经把 lmq8267 的 zerotier.sh 放在了 Makefile 同级目录）
	$(ROMFSINST) $(THISDIR)/zerotier.sh /usr/bin/zerotier.sh
	
	# 3. 创建软链接（可以直接拷贝压缩包里的，或者在这里重新创建）
	ln -sf zerotier-one $(ROMFSDIR)/usr/bin/zerotier-cli
	ln -sf zerotier-one $(ROMFSDIR)/usr/bin/zerotier-idtool
