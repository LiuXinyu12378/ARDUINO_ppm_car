# ARDUINO_ppm_car

            //read PPM signals from 2 channels of an RC reciever
      //http://arduino.cc/forum/index.php/topic,42286.0.html

      //接收机两个通道分别接arduino的数字口2、3脚
      //Most Arduino boards have two external interrupts: numbers 0 (on digital pin 2) and 1 (on digital pin 3). 
      //The Arduino Mega has an additional four: numbers 2 (pin 21), 3 (pin 20), 4 (pin 19), and 5 (pin 18).
      int ppm1 = 2;  
      int ppm2 = 3;

      unsigned long rc1_PulseStartTicks,rc2_PulseStartTicks;        
      volatile int rc1_val, rc2_val;  

      void setup() {

              Serial.begin(9600);  

              //PPM inputs from RC receiver
              pinMode(ppm1, INPUT); 
              pinMode(ppm2, INPUT); 
              pinMode(9, OUTPUT);
              pinMode(11, OUTPUT);
              // 电平变化即触发中断
              attachInterrupt(0, rc1, CHANGE);    
              attachInterrupt(1, rc2, CHANGE);    
      }

      void rc1()
      {
              // did the pin change to high or low?
              if (digitalRead( ppm1 ) == HIGH)
                      rc1_PulseStartTicks = micros();    // store the current micros() value
              else
                      rc1_val = micros() - rc1_PulseStartTicks; 
      }

      void rc2()
      {
              // did the pin change to high or low?
              if (digitalRead( ppm2 ) == HIGH)
                      rc2_PulseStartTicks = micros();    
              else
                      rc2_val = micros() - rc2_PulseStartTicks; 
      }

      void loop() {

              //print values 
              if(rc1_val<1458)
              {analogWrite(9, (1498-rc1_val)*0.6);analogWrite(11, 0);
              Serial.println((1498-rc1_val)*0.6); }
              else if(rc1_val>1518) 
              {analogWrite(11, (rc1_val-1468)*0.6);analogWrite(9, 0);
              Serial.println((rc1_val-1468)*0.6); }
              else
              {analogWrite(11, 0);
              analogWrite(9, 0);
              Serial.println(0);
              }
      //        Serial.print("        ");
      //        Serial.print("channel 2:  ");   
      //        Serial.println(rc2_val);  
      }
