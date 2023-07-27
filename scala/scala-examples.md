[Home](../README.md) > [Scala](scala-examples.md)

## Scala Examples
Some example of simple applications in Scala implementing and using language's various features!

### StringGame Example (Various Scala Features)
Just a scala example:
```scala
 object Main {
    
      def game(f: => Unit): String => Unit  = {
        var msg1 = ""
        var msg2 = ""
        var i = 1
    
        val player1 = msg1 = (_ : String)       //_ lambda
        val player2 = msg2 = (_ : String)
        def rule : Boolean = {msg2 == msg1}     //No param func
        val play = (s1 : String) => (s2 : String) => {      //Currying
          player1(s1)
          player2(s2)
          if(rule)
            f   //Native Call By Name
        }
        var state = (s:String) => {}
    
        return (s:String) => {
          if (i == 1){
            state = play(s)
            i = 2
          }else{
            state(s)
            i = 1
          }
        }
    
      }
    
      def main(args: Array[String]): Unit = {
        val play = game(System.exit(99))
        
        play("hello")
        play("hello") //Exited with 99
      }
      
 }
```