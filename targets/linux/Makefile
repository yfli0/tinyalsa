#改成自己的SDK目录
SDK_PATH = /work/tina_app/aiui/OpenWrt-SDK
CC = $(SDK_PATH)/staging_dir/toolchain-arm_cortex-a7+neon_gcc-4.8-linaro_uClibc-0.9.33.2_eabi/bin/arm-openwrt-linux-gcc
CXX= $(SDK_PATH)/staging_dir/toolchain-arm_cortex-a7+neon_gcc-4.8-linaro_uClibc-0.9.33.2_eabi/bin/arm-openwrt-linux-g++
 
LIBS += -lasound -lpthread -ldl 
#-lcae 
MW_DIR= ../../source
 
TARGET = record
INCLUDE += -I. -I $(SDK_PATH)/staging_dir/target-arm_cortex-a7+neon_uClibc-0.9.33.2_eabi/usr/include/ -I ../../source/include
CFLAGS += -g -fPIC -lpthread -O3 -fno-strict-aliasing -fprefetch-loop-arrays  
CXXFLAGS += -g -fPIC -lpthread -O3 -fno-strict-aliasing -fprefetch-loop-arrays 
 
 

LDFLAGS += -fPIC -L $(SDK_PATH)/staging_dir/target-arm_cortex-a7+neon_uClibc-0.9.33.2_eabi/usr/lib/  -L ../../libs

#找到所有编译目录下的头文件--
SAMDIR=$(MW_DIR)

SAMINCH=$(shell find $(SAMDIR) -name "*.h*")

#找到了，大家可以看到Makefile是可以使用shell命令的，现在结果存储在SAMINCH。

TMP_DIR=$(dir $(SAMINCH))

#dir是个函数，就是列出SAMINCH目录前缀（/*.h之前的字串）的结果。

SAMINCDIR=$(sort $(TMP_DIR))

#sort也是个函数，这是为了去除重复的目录名。

INCLUDE += $(foreach temp, $(SAMINCDIR), -I$(temp))

#只是列出所有匹配的目录结果， 加到-I选项之后，用空格隔开。


#好了，轮到源程序了。

CPP_FILES=$(shell find $(SAMDIR)  -name '*.cpp')
C_FILES =$(shell find $(SAMDIR) -name '*.c')

OBJCPP=$(CPP_FILES:%.cpp=%.o)
OBJC  =$(C_FILES:%.c=%.o)

#建立规则
#$(warning objcpp is $(OBJCPP) objc is $(OBJC) )
#$(warning cflags is $(CFLAGS))
#下面实际的编译过程加上：
$(TARGET): $(OBJCPP) $(OBJC)
	$(CXX) $(LDFLAGS) $(OBJCPP) $(OBJC) -o $@  $(LIBS)

$(OBJCPP): %.o: %.cpp
	$(CXX) -c $< $(CXXFLAGS) $(INCLUDE)  -o $@ 

$(OBJC): %.o: %.c
	$(CC) -c $(CFLAGS) $(INCLUDE) $< -o $@ 


clean:
	rm -fr $(OBJCPP) $(OBJC) $(TARGET)


