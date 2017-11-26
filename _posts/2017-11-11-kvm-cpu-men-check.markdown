---
layout:     post
title:      "kvm物理机资源检查脚本"
subtitle:   "工具"
date:       2017-11-11
author:     "xxx"
header-img: "img/post-bg-unix-linux.jpg"
catalog: true
tags:
    - KVM
    - 虚拟化
    - Linux
    - Scripts
---


```
#!/bin/bash

########################################################
#  用于检查kvm物理主机资源（cpu/内存/磁盘）是否超额配置
# 
# Author: LaoTan 
# Date: 20171111
#########################################################



###0、判断当前服务器是否是kvm
if ! ps aux |grep -v grep| grep -q qemu-kvm ;then
	echo ""I'm not kvm, bye""
	exit
fi



###1、取物理主机总内存和cpu核数
total_mem=$(free |grep ^Mem|awk '{print $2}')
total_cpu=$(cat /proc/cpuinfo |grep -c processor)




###2、根据虚拟机配置文件查出分配的虚拟机内存/CPU/磁盘总数
cd /etc/libvirt/qemu
#使用关联数组，必须将变量指定为数组类型
declare -A rdisk
echo -------------------------------------------------------------------------

for i in $(ls *.xml)
do
	count=$((count+1))
	v_name=$(grep '<name>' $i|awk -F'<|>' '{print $3}')
        if virsh list | grep -wq $v_name;then
        	mem[$count]=$(grep ""memory""  $i|awk -F'<|>' '{print $3}')
	        v_mem=$((mem[$count]+v_mem))	

	        cpu[$count]=$(grep ""vcpu""  $i|awk -F'<|>' '{print $3}')
	        v_cpu=$((cpu[$count]+v_cpu))
        
	        #虚拟磁盘，跳过全盘虚拟化情况。并且需要考虑1台虚拟机有多个虚拟磁盘
		for j in $(grep ""source file""  $i|awk -F""'"" '{print $2}')
		do
			disk[$count]=$j
			[[ -z ${disk[$count]} ]] && continue
			rd=$(dirname ${disk[$count]}|sed 's; ;;g')
			rd_list=""$rd $rd_list""
			disk_size[$count]=$(qemu-img info ${disk[$count]}|grep ^virtual |awk '{print $4}'|sed 's;(;;')
			rdisk[""${rd/\//}""]=$((disk_size[$count]+rdisk[""${rd/\//}""]))
			rdisk[$count]=$((rdisk[$count]+disk_size[$count]))
		done
	fi
	echo -e ""name=$v_name\t\tmem=$((mem[$count]/1024/1000))GB\t\tcpu=${cpu[$count]}\t\tdisk=$((rdisk[$count]/1024/1024/1024))GB""
done



###3、对比内存/CPU/磁盘物理总数和分配总数，检查是否有超配情况
#内存
if [[ $total_mem -gt $v_mem ]];then
	mem_res=""ok""
else
	mem_res=""err""
fi
#CPU
if [[ $total_cpu -gt $v_cpu ]];then
	cpu_res=""ok""
else
	cpu_res=""err""
fi
#磁盘
for i in $(echo $rd_list|tr "" "" ""\n""|sort|uniq )
do
	rsize=$(df $i |grep ^/|awk '{print $3+$4}')
	if [[ $((rsize*1024)) -gt ${rdisk[${i/\//}]} ]];then
        	disk_res=""$disk_res (${i}:$(((rsize*1024)/1024/1024/1024))GB/$((rdisk[${i/\//}]/1024/1024/1024))GB)ok""
	else
        	disk_res=""$disk_res (${i}:$(((rsize*1024)/1024/1024/1024))GB/$((rdisk[${i/\//}]/1024/1024/1024))GB)err""
	fi
done




###4、判断运行结果
if echo ""mem_res=$mem_res\tcpu_res=$cpu_res\tdisk_res=$disk_res""|grep -q err ;then
	result=""FAIL.""
else
	result=""OK.""
fi




###5、格式化输出，输出示例：""OK |mem_res=(132110092/81949696)ok		cpu_res=(32/30)ok	disk_res= /kvm_ok""
echo -------------------------------------------------------------------------
echo -e ""$result |mem_res=($(((total_mem/1024)/1000))GB/$(((v_mem/1024)/1000))GB)$mem_res\tcpu_res=($total_cpu/$v_cpu)$cpu_res\tdisk_res=$disk_res""
```