#
```kt
// request
        #
     ##
    ###
   ####
  #####
 ######

// nomal way
fun main() {
   
    for(i in 6 downTo 1){
      for(y in 0 until 6){
           if(i <= y){
               print("#")
           }else{
               print(" ")
           }
      }
      print("\n")
    }
}
// another way
fun main() {
    for(i in 1 .. 6){
      val space = " ".repeat(6-i)
      val symbol = "#".repeat(i)
      println("$space $symbol")
    }
}
```