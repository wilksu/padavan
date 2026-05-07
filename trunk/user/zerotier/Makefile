# 定义名称
SRC_NAME = zerotier-one
# 压缩包解压后生成的目录名
SRC_DIR = zerotier-one
# 压缩包下载地址
SRC_URL = https://github.com/lmq8267/ZeroTierOne/releases/download/1.16.0/zerotier.tar.gz

THISDIR = $(shell pwd)
# 真正的二进制文件路径（在解压后的目录里）
SRC_BIN_PATH = $(THISDIR)/$(SRC_DIR)/zerotier-one

# 默认目标
all: download_test extract_test
	@echo "ZeroTier 预编译包处理完成。"

# 下载逻辑：如果不存在则下载
download_test:
	( if [ ! -f zerotier.tar.gz ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O zerotier.tar.gz $(SRC_URL); \
	fi )

# 解压逻辑：如果目录不存在则解压，并赋予执行权限
extract_test:
	if [ ! -d $(SRC_DIR) ] ; then \
		tar -xzvf zerotier.tar.gz ; \
	fi ;
	if [ -f $(SRC_BIN_PATH) ] ; then \
		chmod +x $(SRC_BIN_PATH) ; \
	fi ;

# 清理逻辑：删除所有生成物
clean:
	rm -rf $(SRC_DIR) zerotier.tar.gz zerotier-cli zerotier-idtool

# 安装到固件镜像逻辑
romfs:
	# 1. 强制清理固件目标目录中可能存在的旧脏数据（防止目录/文件冲突）
	rm -rf $(ROMFSDIR)/usr/bin/zerotier-one
	rm -f $(ROMFSDIR)/usr/bin/zerotier-cli
	rm -f $(ROMFSDIR)/usr/bin/zerotier-idtool
	rm -f $(ROMFSDIR)/usr/bin/zerotier.sh

	# 2. 安装真正的二进制文件（从解压目录里提取文件）
	# 注意：这里必须指定源文件全路径，安装为 /usr/bin/zerotier-one 这个文件
	$(ROMFSINST) $(SRC_BIN_PATH) /usr/bin/zerotier-one
	
	# 3. 安装启动脚本
	$(ROMFSINST) $(THISDIR)/zerotier.sh /usr/bin/zerotier.sh
	
	# 4. 建立软链接（在固件镜像的 /usr/bin 目录下建立相对链接）
	# 使用子 shell 进入目标目录操作，确保链接目标 zerotier-one 指向的是文件
	( cd $(ROMFSDIR)/usr/bin && ln -sf zerotier-one zerotier-cli )
	( cd $(ROMFSDIR)/usr/bin && ln -sf zerotier-one zerotier-idtool )
