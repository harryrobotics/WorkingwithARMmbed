# Working with ARMmbed

##Serial port

`stdio (printf, putc, getc, etc), malloc` & `new` in ISR

Because of the mutexes in the ARM C standard library you cannot use stdio (printf, putc, getc, etc), malloc and new in ISR! Therefore, serial interrupt will not work in RTOS.
Therefore, to solve this issue, mbed developed RawSerial.

Sample code for communication between 2 microcontroller - Receiving side:

```
void receiver_thread(void const *n);
RawSerial receiver(TX_port,RX_port); //Need to define TX_port and RX_port
char buffer_data[20];

int main(){
    //Define the thread and start
    Thread receiver_task(receiver_thread, NULL, osPriorityNormal);
    while(1){
    }
    
    return 0;
}

void receiver_thread(void const *n){
      receiver.baud(115200);
      receiver.attach(&rxCallback, RawSerial::RxIrq);
      while(1){
      }
      Thread::wait(100);
}

void rxCallback()
     while(receiver.getc() != '$'){    //The $ sign is start of the package
     }
    for(int i = 0 ; i < sizeof(buffer_data); i ++){
    buffer_data[i] = receiver.getc();
    if(buffer_data[i] == '\n'){         // The \n sign is end of the package
        break;
    }
    }
}

```
   
