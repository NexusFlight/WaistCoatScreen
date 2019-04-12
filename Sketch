#include <Wire.h>
#include <Adafruit_SSD1306.h>
#include <Adafruit_GFX.h>


int upCharPin = 2;
int downCharPin = 3;
int nextCharPin = 4;
int prevCharPin = 5;
int upFontPin = 6;
int downFontPin = 7;
int editPin = 8;

int upCState = 0;
int downCState = 0;
int nextCState = 0;
int prevCState = 0;
int upFState = 0;
int downFState = 0;
int editState = 0;

#define OLED_RESET 4
Adafruit_SSD1306 display(OLED_RESET);

void setup() {
	pinMode(upCharPin, INPUT_PULLUP);
	pinMode(downCharPin, INPUT_PULLUP);
	pinMode(nextCharPin, INPUT_PULLUP);
	pinMode(prevCharPin, INPUT_PULLUP);
	pinMode(upFontPin, INPUT_PULLUP);
	pinMode(downFontPin, INPUT_PULLUP);
	pinMode(editPin, INPUT_PULLUP);

	Wire.begin();
	display.begin(SSD1306_SWITCHCAPVCC, 0x3C);
}


int aSelector = 0;
String alphabet = "AaBbCcDdEeFfGgHhIiJjKkLlMmNnOoPpQqRrSsTtUuVvWwXxYyZz !?,.'#";
int cSelector = 0;
String content = "hello";
bool editMode = false;
int fontSize = 2;

void loop() {
	upCState = digitalRead(upCharPin);
	downCState = digitalRead(downCharPin);
	nextCState = digitalRead(nextCharPin);
	prevCState = digitalRead(prevCharPin);
	upFState = digitalRead(upFontPin);
	downFState = digitalRead(downFontPin);
	editState = digitalRead(editPin);

	if (editState == LOW) {
		editMode = !editMode;
		display.clearDisplay();
		delayMicroseconds(1000);
	}
	if (editMode) {
		display.invertDisplay(true);

		if (nextCState == LOW) {
			if (cSelector == content.length() - 1)
			{
				content = content + "b";
			}
			cSelector++;
		}
		if (prevCState == LOW) {
			cSelector--;
			if (cSelector < 0) {
				cSelector = 0;
			}
		}
		if (upFState == LOW) {
			fontSize++;
			if (fontSize > 21) {
				fontSize = 21;
			}
		}
		if (downFState == LOW) {
			fontSize--;
			if (fontSize < 0) {
				fontSize = 0;
			}
		}

		if (downCState == LOW) {
			aSelector--;
			if (aSelector < 0) {
				aSelector = alphabet.length();
			}
		}else if (upCState == LOW) {
			aSelector++;
			if (aSelector > alphabet.length()) {
				aSelector = 0;
			}
		}
		else {
			for (size_t i = 0; i < alphabet.length(); i++)
			{
				if (content[cSelector] == alphabet[i]) {
					aSelector = i;
					break;
				}
			}
		}

		content[cSelector] = alphabet[aSelector];
	}
	else {
		display.invertDisplay(false);
		cSelector = 0;
		aSelector = 0;
	}

	display.clearDisplay();
	display.setTextColor(WHITE);
	display.setTextSize(fontSize);
	display.setCursor(0, 0);
	display.print(content);

	display.display();
}
