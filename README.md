# breakout
unfinished breakout

#include <allegro5/allegro.h>
#include<allegro5/allegro_primitives.h>

class Brick {
private:
	int width= 0;
	int height= 0;
	int xPos= 0;
	int yPos= 0;
	int dead= 0;

public:
	void constructor();
	void initBrick(int x, int y);
	void draw();
	void isDead();
	void killBrick();	
	bool collide(int bx, int by, int bw, int bh);
};
//constants: variables that shouldn't change once the game starts
const float FPS = 60;
const int SCREEN_W = 1000;
const int SCREEN_H = 1000;

//enumeration to help you remember what numbers represent which directions
enum MYKEYS {
	KEY_LEFT, KEY_RIGHT
};
////function declaration
bool collide(int bx, int by, int bw, int bh, int px, int py, int pw, int ph);

int main()
{
	///////////////////////////////////needed for all games!////////////////
	al_init();
	al_install_keyboard();
	al_init_primitives_addon();

	//set up game screen, event queue, and timer
	ALLEGRO_DISPLAY* display = al_create_display(SCREEN_W, SCREEN_H);
	ALLEGRO_EVENT_QUEUE* event_queue = al_create_event_queue();
	ALLEGRO_TIMER* timer = al_create_timer(1.0 / 60);

	//tell the event queue what to listen for
	al_register_event_source(event_queue, al_get_display_event_source(display));
	al_register_event_source(event_queue, al_get_timer_event_source(timer));
	al_register_event_source(event_queue, al_get_keyboard_event_source());
	al_start_timer(timer);

	//////////////////////////////////////////////////////////////////////////////

	//paddle 1 starting position
	double xPos = 600;
	double yPos = 950;
	
	//game variables
	bool key[4] = { false, false, false, false }; //holds key clicks
	bool redraw = true; //variable needed for render section
	bool doexit = false;

	//ball starting position
	float ball_x = 20;
	float ball_y = 200;

	//ball speed
	float ball_dx = 4.0, ball_dy = 4.0;
	///////////////////////////////////Bricks
	Brick  b1;
	b1.initBrick(50, 50);
	Brick  b2;
	b2.initBrick(200, 50);
	Brick  b3;
	b3.initBrick(350, 50);
	Brick  b4;
	b4.initBrick(500, 50);
	Brick  b5;
	b5.initBrick(650, 50);
	Brick  b6;
	b6.initBrick(800, 50);
	Brick  b7;
	b7.initBrick(50, 150);
	Brick b8;
	b8.initBrick(200, 150);
	Brick b9;
	b9.initBrick(350, 150);
	Brick b10;
	b10.initBrick(500, 150);
	Brick b11;
	b11.initBrick(650, 150);
	Brick b12;
	b12.initBrick(800, 150);
	Brick b13;
	b13.initBrick(50, 250);
	Brick b14;
	b14.initBrick(200, 250);
	Brick b15;
	b15.initBrick(350, 250);
	Brick b16;
	b16.initBrick(500, 250);
	Brick b17;
	b17.initBrick(650, 250);
	Brick b18;
	b18.initBrick(800, 250);
	Brick b19;
	b19.initBrick(50, 350);
	Brick b20;
	b20.initBrick(200, 350);
	Brick b21;
	b21.initBrick(350, 350);
	Brick b22;
	b22.initBrick(500, 350);
	Brick b23;
	b23.initBrick(650, 350);
	Brick b24;
	b24.initBrick(800, 350);

	while (1) {
		ALLEGRO_EVENT ev;
		al_wait_for_event(event_queue, &ev);

		//TIMER  (handles physics)
		//collisions with wall
		if (ev.type == ALLEGRO_EVENT_TIMER) {

			//MOVE
			ball_x += ball_dx;
			ball_y += ball_dy;

			if (ball_x < 0 || ball_x > 1000) { //bounces against side walls
				ball_dx = -ball_dx;
			}

			if (ball_y < 0 || ball_y > 1000) { //bounce against top and bottom walls
				ball_dy = -ball_dy;
			}

			//collision with paddels
			if (collide(ball_x, ball_y, 30, 30, xPos, yPos, 60, 80))
				ball_dy = -ball_dy;//reflection
			//collision with bricks





			//move player 4 pixels in a direction when key is pressed
			if (key[KEY_LEFT]) {
				xPos -= 6.0;
			}
			if (key[KEY_RIGHT]) {
				xPos += 6.0;
			}
	
			redraw = true;
		}
		////lets you close with the x button
		else if (ev.type == ALLEGRO_EVENT_DISPLAY_CLOSE) {
			break;
		}
		else if (ev.type == ALLEGRO_EVENT_KEY_DOWN) {
			switch (ev.keyboard.keycode) {
			case ALLEGRO_KEY_LEFT:
				key[KEY_LEFT] = true;
				break;
			case ALLEGRO_KEY_RIGHT:
				key[KEY_RIGHT] = true;
				break;
			}
		}
		else if (ev.type == ALLEGRO_EVENT_KEY_UP) {
			switch (ev.keyboard.keycode) {
			case ALLEGRO_KEY_LEFT:
				key[KEY_LEFT] = false;
				break;
			case ALLEGRO_KEY_RIGHT:
				key[KEY_RIGHT] = false;
				break;
			}
		}

		//RENDER SECTION
		if (redraw && al_is_event_queue_empty(event_queue)) {
			redraw = false;

			al_clear_to_color(al_map_rgb(0, 0, 0)); //clears the screen

			al_draw_filled_circle(ball_x, ball_y, 20, al_map_rgb(200, 200, 100));

			al_draw_rectangle(xPos, yPos, xPos + 80, yPos, al_map_rgb(50, 120, 10), 20); //draw the player
			
			//////////////////////////////////////////draw the bricks
			b1.draw();
			b2.draw();
			b3.draw();
			b4.draw();
			b5.draw();
			b6.draw();
			b7.draw();
			b8.draw();
			b9.draw();
			b10.draw();
			b11.draw();
			b12.draw();
			b13.draw();
			b14.draw();
			b15.draw();
			b16.draw();
			b17.draw();
			b18.draw();
			b19.draw();
			b20.draw();
			b21.draw();
			b22.draw();
			b23.draw();
			b24.draw();
			al_flip_display();

		}

	}//end game loop


	al_destroy_timer(timer);
	al_destroy_display(display);
	al_destroy_event_queue(event_queue);

	return 0;
}

////function definitions
bool collide(int bx, int by, int bw, int bh, int px, int py, int pw, int ph) {
	if ((bx + bw > px) &&  //did the right edge of the ball hit the 
		(bx < px + pw) &&//did the left edge of the ball
		(by + bh > py) &&
		(by < py + ph))
		return true;
	else
		return false;

}
void Brick::initBrick(int x, int y) {
	xPos = x;
	yPos = y;
	width = 100;
	height = 20;
	dead = false;
}
void Brick::constructor(){
	int width=0;
	int height=0;
	int xPos=0;
	int yPos=0;
	int dead=0;
}

void Brick:: draw() {
	al_draw_filled_rectangle(xPos, yPos, xPos + width, yPos + height, al_map_rgb(200, 100, 150));

}
