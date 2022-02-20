#define up A4
#define down A3
#define automanual A2
#define ledAuto A1
#define ledManual A0
#define led 13
#include<Stepper.h>
int previous = 0;
int val = 0;

const int stepsPerRevolution = 200;

Stepper myStepper(stepsPerRevolution, 4, 5, 6, 7);

void setup()
{
    myStepper.setSpeed(5);
    pinMode(up, INPUT);
    pinMode(down, INPUT);
    pinMode(automanual, INPUT);
    pinMode(ledAuto, OUTPUT);
    pinMode(ledManual, OUTPUT);
    pinMode(led, OUTPUT);
    Serial.begin(9600);
}

void loop()
{
    // step one revolution  in one direction:
    if (digitalRead(automanual) == LOW)
    {
        digitalWrite(ledManual, HIGH);
        digitalWrite(ledAuto, LOW);
        digitalWrite(led, LOW);
        if (digitalRead(up) == LOW)
        {
            myStepper.step(1);
            while (digitalRead(up) == LOW)
                ;
            delay(100);
        }
        else if (digitalRead(down) == LOW)
        {
            myStepper.step(-1);
            while (digitalRead(down) == LOW)
                ;
            delay(100);
        }
    }

    else
    {
        digitalWrite(ledManual, LOW);
        digitalWrite(ledAuto, HIGH);
        val = (analogRead(5) / 20);
        if (val < 5)
        {
            digitalWrite(led, (!digitalRead(led)));
            delay(500);
        }
        else
        {
            digitalWrite(led, LOW);
        }
        myStepper.step(val - previous);
        previous = val;
    }
}
