# streaming \n

# run this on spark-shell \n

import org.apache.spark.streaming.{Duration, Seconds, StreamingContext} \n
import org.apache.spark.storage._
val ssc=new StreamingContext(sc,Seconds(10))
val lines=ssc.socketTextStream("<<SERVER_NAME>>",3333,StorageLevel.MEMORY_AND_DISK_SER)
val words=lines.flatMap(_.split(" "))
val pairs=words.map(word=>(word,1))
val wc=pairs.reduceByKey(_+_)
val newwc=wc.map((x)=>(x._2,x._1))
val newwc_red=newwc.reduceByKey(_+","+_)
newwc_red.print()
ssc.start()
ssc.awaitTermination()

#publish streaming from linux terminal to specified <<SERVER_NAME>>

nc -l 3333