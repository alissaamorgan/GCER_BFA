# GCER_BFA
#define rotate 0
#define shoulder 2//both red on top
//looking at face of Wallaby,negative is clockwise, positive is counterclockwise
//looking down on create, positive on rotate is clockwise
//accel_y roughly 0 at horizontal
//accel_yroughly 1000 at vertical- those defines start next line
//accel_y roughly at -625 to be all the way down - make sure claw is not in way
#define horizontal 0
#define vertical 1000
#define claw 3
//#define claw_open 1190//to get this value, subtract 760 from claw_close
#define claw_closed 1020//if claw breaks, reset to close position and take this value from Wallaby
#define wrist 1
#define horizontal_with_arm 1527
#define rotate_clicker 0
#define wrist_for_front_pickup 1970
#define shoulder_start_position 1135//was 1135
#define shoulder_up 100
#define shoulder_for_bulldozing 1730 //1711
#define wrist_dump 400
#define wrist_delivery 300
int claw_barely_open = claw_closed - 420;
int claw_bucket_width = claw_closed - 620;
int claw_for_frisbee1 = claw_closed - 340;
#define shoulder_for_frisbee 1150
#define wrist_for_frisbee 1450
#define wrist_with_frisbee 1650
int claw_open = claw_closed -760;
void set_servo_slow(int serv, int desired_posi)
{
enable_servo(serv);
get_servo_position(serv);
if (get_servo_position(serv)>desired_posi)
{
    	int current_posi = get_servo_position(serv);
    	while (get_servo_position(serv)>desired_posi)
    	{
        	current_posi = current_posi-3;
        	set_servo_position(serv,current_posi);
        	msleep(2);
    	}
}
else
{
    	int current_posi = get_servo_position(serv);
    	while (get_servo_position(serv)<desired_posi)
    	{
        	current_posi = current_posi+3;
        	set_servo_position(serv,current_posi);
        	msleep(2);
    	}
}
}
void pick_frisbee_up()
{
set_servo_slow(claw,claw_for_frisbee1);
set_servo_slow(wrist,wrist_with_frisbee);
}
void frisbee_approach_positions()
{
set_servo_slow(shoulder,shoulder_for_frisbee);
set_servo_slow(wrist,wrist_for_frisbee);  
set_servo_slow(claw,claw_barely_open);
}
void claw_to_delivery2()//1st dump,2nd step
{
int delivery_counter3 = claw_barely_open;
while (delivery_counter3>claw_bucket_width)
{
    	delivery_counter3=delivery_counter3 - 1;
    	set_servo_position(claw, delivery_counter3);
    	msleep(2);
}
}
void claw_to_delivery()//1st dump,1st step
{
int delivery_counter2 = claw_closed;
while (delivery_counter2>claw_barely_open)
{
    	delivery_counter2=delivery_counter2 - 1;
    	set_servo_position(claw, delivery_counter2);
    	msleep(2);
}
}
void wrist_to_delivery()//1st dump
{
int delivery_counter = wrist_dump;
while (delivery_counter>50)
{
    	delivery_counter=delivery_counter - 1;
    	set_servo_position(wrist, delivery_counter);
    	msleep(2);
}
}
void perfect_turn()//used to adjust for pom drop off
{
create_drive_direct(50, 10);
msleep(400);
create_stop();
create_drive_direct(-20,60);
msleep(2100);
create_stop();
}
void CCW(int timer)
{
create_drive_direct(-200, 200);
msleep(timer);
create_stop();
}
void CW(int timer)
{
create_drive_direct(200, -200);
msleep(timer);
create_stop();
}
void doze_to_dump()
{
int shoulder_counter = shoulder_for_bulldozing;
int wrist_counter = wrist_for_front_pickup;
while (shoulder_counter>shoulder_up || wrist_counter>wrist_dump)//loop exits when it gets to dump position
{
    	if(shoulder_counter>shoulder_up)
    	{
        	shoulder_counter = shoulder_counter - 1;
        	set_servo_position(shoulder,shoulder_counter);
        	msleep(1);
    	}
    	if(wrist_counter>wrist_dump)
    	{
        	wrist_counter= wrist_counter - 1;
        	set_servo_position(wrist,wrist_counter);
        	msleep(1);
    	}
}
}
void dump_to_doze()
{
int shoulder_counter = get_servo_position(shoulder);
int wrist_counter = get_servo_position(wrist);
while (shoulder_counter<shoulder_for_bulldozing || wrist_counter<wrist_for_front_pickup)//loop exits when it gets to doze position
{
    	if(shoulder_counter<shoulder_for_bulldozing)
    	{
        	shoulder_counter = shoulder_counter + 1;
        	set_servo_position(shoulder,shoulder_counter);
        	msleep(1);
    	}
    	if(wrist_counter<wrist_for_front_pickup)
    	{
        	wrist_counter= wrist_counter + 2;
        	set_servo_position(wrist,wrist_counter);
        	msleep(1);
    	}
}
}
void claw_slow_close()
{
int ser_pos = get_servo_position(claw);//variable to hold current servo position
while (ser_pos < claw_closed)//loop exits when it gets to closed
{
    	ser_pos = ser_pos + 1;//move servo one position	
    	set_servo_position(claw,ser_pos);
    	msleep(2);//time between ticks of servo
}
}
void create_forward3(int timer)//slow forward for dumping 1st time
{
create_drive_direct(50, 50);
msleep(timer);
create_stop();
}
void create_forward(int timer)
{
create_drive_direct(200, 200);
msleep(timer);
create_stop();
}
void create_forward2(int timer)//for getting to dumping site,arc slightly away from wall
{
create_drive_direct(440, 390);
msleep(timer);
create_stop();
}
void create_backward(int timer)
{
create_drive_direct(-200, -200);
msleep(timer);
create_stop();
}
void shoulder_doze()
{
int posi= 1135;
enable_servo(shoulder);
while(posi < shoulder_for_bulldozing)
{
    	set_servo_position(shoulder,posi);
    	posi=posi+1;
    	msleep(3);
}
}
void shoulder_start()
{
enable_servo(shoulder);
set_servo_position(shoulder,shoulder_start_position);
}
void wrist_start()
{
enable_servo(wrist);
set_servo_position(wrist,322);
}
void claw_start()
{
enable_servo(claw);
set_servo_position(claw,claw_closed);
}
void wrist_horizontal()
{
enable_servo(wrist);
set_servo_position(wrist,wrist_for_front_pickup);

}
void move_to_reset_clicker()
{
mav(rotate,-300);
while(digital(rotate_clicker)==0){}
mav(rotate,0);
cmpc(rotate);
}
void rotate_counterclockwise(int ticks)
{
mav(rotate,200);
while(gmpc(rotate)<ticks){}
mav(rotate,0);
}
void move_wrist_to(int desired_angle)
{
double ticks_from_center = (horizontal_with_arm + (desired_angle * 11.377));
int servo_postion = round (ticks_from_center);
set_servo_position (wrist, servo_postion);
}
void move_claw(int status)
{
enable_servos();
set_servo_position(claw,status);
msleep(200);
}
int buffer_y()
{
int average = 0;
int counter = 0;
while (counter <= 6)
{
    	average = average + accel_y();
    	counter = counter + 1;
    	msleep(1);
}
average = average / 6;
return average;
}
/*
void set_shoulder_to_angle(int set_angle)
//max angle = 90
//min angle =-56
//each angle should correspond to a accel_y value of approximately 1000/90 or 11.1
{
int y_accel_value = set_angle * 11.1;
if (y_accel_value > 1000){y_accel_value = 1000;}
if (y_accel_value < -625){y_accel_value = -625;}
set_angle = y_accel_value * 11.1;
    	while (buffer_y() != y_accel_value)
    	{  
        	printf("%d\n",y_accel_value);
            	mav(shoulder,(buffer_y() - y_accel_value));
        	 if (buffer_y() > (y_accel_value-5) && buffer_y() < (y_accel_value+5)){break;}
    	}
mav(shoulder, 0);
msleep (2);
}
*/
void collect_poms()
{
create_drive_direct (80,80);
msleep(500);
create_stop();
create_drive_direct (0,-80);
msleep(100);
create_drive_direct (-80,-80);
msleep(150);
}
