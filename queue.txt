pipeline {
   agent any
   stages {
      stage('Hello') {
         steps {
            script{
                max = 4
                latch = new java.util.concurrent.LinkedBlockingDeque(max)
                for(int i=0; i<max; i++)
                    latch.offer("$i")
                    
                def l=(1..15).toList()
                def b=[:]
                l.each{
                    b[it]={
                        def thing = null
                        waitUntil {
                           script {
                                thing = latch.pollFirst()
                                return thing != null
                           }
                        }
                        try{
                            stage("$it"){
                                node('master'){
                                    println it
                                    println env.Node
                                    sleep 10
                                }
                            }
                        }
                        finally{
                            latch.offer(thing)
                        }
                    }
                }
                
                timestamps{
                    parallel b
                }
            }
         }
      }
   }
}
