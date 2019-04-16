이름 : 고예나 
사번 : 09953
부서 : 제조자동화 O.G

문제 풀이 

1. hdfs dfs -put /loudacre/devicestatus.txt
	=> put file into hdfs

2. devicetxt = sc.textFile("/loudacre/devicestatus.txt") 
	=> put the textfile in hdfs into python variable

3. devicetxt = devicetxt.filter(lambda line : len(line)>20)
	=> filter the lines to get rid of lines which`s length are equal or less then 20 because we will split the line using character of line[19:20]

4. devicetxt.map(lambda line: line.split(line[19:20])).filter(lambda values: len(values)==14)
	=> filter each line which has 14 values 

5. resDeviceTxt = editDeviceTxt.map(lambda value : value[0], value[1].split(' ')[0], values[2]. value[12], value[13]))
	=> rearrange the value with the given format

