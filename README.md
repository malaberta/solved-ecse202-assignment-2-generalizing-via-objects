Download Link: https://assignmentchef.com/product/solved-ecse202-assignment-2-generalizing-via-objects
<br>
The goal of this assignment is to build a simulation for 100 balls with randomly chosen parameters.  All balls are launched from the same X coordinate (center of the field) with initial height corresponding to ball radius.  Figure 1 shows several snapshots from a video sequence generated by this simulation.










Figure 1 – a sequence of 3 frames from a video of the simulation sequence.




At minimum, you will need at least 2 classes for this simulation, the aBall class which generates an instance of ball in motion according to Assignment 1, and the bSim class which sets up the display, generates parameters and an instance of each ball, and runs until terminated by the user.




The first task is to design the aBall class according to the following constructor:




public aBall (double Xi, double Yi, double Vo, double theta,             double bSize, Color bColor, double bLoss) where




<table width="463">

 <tbody>

  <tr>

   <td width="39">Xi:</td>

   <td width="49"> </td>

   <td colspan="2" width="375">is the X coordinate of the initial launch position (meters)</td>

   <td width="95"> </td>

  </tr>

  <tr>

   <td width="39">Yi:</td>

   <td width="49"> </td>

   <td colspan="2" width="375">is the Y coordinate of the initial launch position (meters)</td>

   <td width="95"> </td>

  </tr>

  <tr>

   <td width="39">Vo:</td>

   <td width="49"> </td>

   <td colspan="2" width="375">is the initial launch velocity (meters/second)</td>

   <td width="95"> </td>

  </tr>

  <tr>

   <td colspan="3" width="96">theta:</td>

   <td colspan="2" width="462">is the initial launch angle (degrees, as measured from the ground plane)</td>

  </tr>

  <tr>

   <td colspan="3" width="96">bSize:</td>

   <td colspan="2" width="462">the radius of the ball (meters)</td>

  </tr>

  <tr>

   <td colspan="3" width="96">bColor:</td>

   <td colspan="2" width="462">ball color</td>

  </tr>

  <tr>

   <td colspan="3" width="96">bLoss:</td>

   <td colspan="2" width="462">energy loss coefficient</td>

  </tr>

  <tr>

   <td width="37"></td>

   <td width="40"></td>

   <td width="7"></td>

   <td width="306"></td>

   <td width="72"></td>

  </tr>

 </tbody>

</table>

<strong> </strong>

This class essentially encapsulates the code you wrote for Assignment 1.




The second task is to write the bSim class which randomly generates the parameters for 100 separate balls.  The parameters used by this class are summarized below.




<table width="616">

 <tbody>

  <tr>

   <td width="384">private static final int WIDTH = 1200;private static final int HEIGHT = 600; private static final int OFFSET = 200;</td>

   <td width="232">// n.b. screen coordinates</td>

  </tr>

  <tr>

   <td width="384">private static final double SCALE = HEIGHT/100;</td>

   <td width="232">// pixels per meter</td>

  </tr>

  <tr>

   <td width="384">private static final int NUMBALLS = 100;</td>

   <td width="232">// # balls to simulate</td>

  </tr>

  <tr>

   <td width="384">private static final double MINSIZE = 1.0;</td>

   <td width="232">// Minumum ball radius (meters)</td>

  </tr>

  <tr>

   <td width="384">private static final double MAXSIZE = 10.0;</td>

   <td width="232">// Maximum ball radius (meters)</td>

  </tr>

  <tr>

   <td width="384">private static final double EMIN = 0.1;</td>

   <td width="232">// Minimum loss coefficient</td>

  </tr>

  <tr>

   <td width="384">private static final double EMAX = 0.6;</td>

   <td width="232">// Maximum loss coefficient</td>

  </tr>

  <tr>

   <td width="384">private static final double VoMIN = 40.0;</td>

   <td width="232">// Minimum velocity (meters/sec)</td>

  </tr>

  <tr>

   <td width="384">private static final double VoMAX = 50.0;</td>

   <td width="232">// Maximum velocity (meters/sec)</td>

  </tr>

  <tr>

   <td width="384">private static final double ThetaMIN = 80.0;</td>

   <td width="232">// Minimum launch angle (degrees)</td>

  </tr>

  <tr>

   <td width="384">private static final double ThetaMAX = 100.0;</td>

   <td width="232">// Maximum launch angle (degrees)</td>

  </tr>

 </tbody>

</table>




Designing the aBall class




The aBall class must do the following:




<ol>

 <li>Create an instance of a GOval from the specified parameters (constructor). Further, since we need to add this object to the display, a corresponding get method needs to be included, i.e., getBall();.</li>

 <li>Once the instance is generated, the simulation loop is run until the ball runs out of steam. Since messages are sent to the GOval inside of aBall, the ball will animate automatically until the simulation terminates.  Most of the code you wrote in Assignment 1 can be copied with little change.</li>

 <li>Since we want each ball to move independently, the corresponding objects must run <em>concurrently</em>. Although this is well beyond the scope of an introductory course, Java makes it very easy to make objects run concurrently.  All that is required is for the aBall to extend the <em>thread</em></li>

</ol>




You can find out more about the thread class here:

<u>https://docs.oracle.com/javase/7/docs/api/java/lang/Thread.html</u>).




The concept of threading will be very important for your later courses (especially Design Principles and Methods, ECSE 211) and allows your application to have multiple threads of execution running concurrently.

<h1>Design Approach</h1>




Let us first consider the aBall class, the template for which is shown below (you are to use this template in designing your implementation:




public class aBall extends Thread {




/**

<ul>

 <li>The constructor specifies the parameters for simulation. They are *</li>

 <li>@param Xi double The initial X position of the center of the ball</li>

 <li>@param Yi double The initial Y position of the center of the ball</li>

 <li>@param Vo double The initial velocity of the ball at launch</li>

 <li>@param theta double Launch angle (with the horizontal plane)</li>

 <li>@param bSize double The radius of the ball in simulation units</li>

 <li>@param bColor Color The initial color of the ball</li>

 <li>@param bLoss double Fraction [0,1] of the energy lost on each bounce */</li>

</ul>

public aBall(double Xi, double Yi, double Vo, double theta, double bSize, Color bColor, double bLoss) {




this.Xi = Xi;                                                          // Get simulation parameters

this.Yi = Yi;            this.Vo = Vo;                     this.theta = theta;                   this.bSize = bSize;                 this.bColor = bColor;

this.bLoss = bLoss;




/**

<ul>

 <li>The run method implements the simulation loop from Assignment 1.</li>

 <li>Once the start method is called on the aBall instance, the</li>

 <li>code in the run method is executed concurrently with the main * program.</li>

 <li>@param void</li>

 <li>@return void</li>

</ul>

*/




public void run() {

// Simulation goes here…

}

}




Example:




Using the <strong>aBall</strong> class, create a simulation for a single ball, initially located at coordinates (simulation, not screen) (10,100), with size=6, Color=Red, loss coefficient=0.25, with an initial velocity of 1.0 m/s. at an angle of 30°.




//

// Code to set up the graphics environment as per Assignment 1

// goes here

//

aBall redBall = new aBall(10.0,100.0,1.0,30.0,6.0,Color.RED,0.25);   add(redBall.getBall());   redBall.start();

Details:




Inside the constructor of the aBall class, an instance of GOval corresponding to a filled circle is instantiated as follows:




myBall = new GOval(paramaters); myBall.setFilled(true);

myBall.setFillColor(parameter);




To make the resulting GOval accessable outside of aBall, we add an appropriate “getter” as follows:




public GOval getBall() {   return myBall;

}




Since aBall is an extension of the Thread class, it inherits the corresponding methods – one of which is “start”.  The effect of calling the start method on the aBall class instance is to call the run method associated with aBall.  However, there is an important side effect to this method call. Rather than returning when the method has completed, it returns <em>immediately</em>, allowing the method to run concurrently with the calling program.  In other words, the simulation embedded within the aBall instance runs in <em>parallel</em> with the main program and all other instances of the aBall class.




Another problem that we have to deal with is that we no longer have access to the pause method, used to control the speed at which the display is updated.  Fortunately the Thread class includes a method called “sleep” which takes an argument of type long expressing the delay in milliseconds.  We haven’t dealt with exceptions yet, so just include the following pattern in place of pause to achieve the same effect.

try {                                                                                            // pause for 50 milliseconds

Thread.sleep(50);

} catch (InterruptedException e) {                            e.printStackTrace();

}




For reasons that will become clear in later courses, the sleep duration for the aBall thread should be half as long as the time step.  For example, if time is updated every 100 mS  (0.1 S), then the sleep duration (above) should be half of that (50 mS).




One more detail regarding the aBall class – stopping.  When the while loop is exited, the run method terminates which results in the thread terminating.  For the while loop to continue running, two conditions must be met:




<ol>

 <li>The total energy (KEx+KEy) must be greater than some minimum energy ETHR.</li>

</ol>

AND

<ol start="2">

 <li>The total energy must be less than it was on the previous bounce.</li>

</ol>




The effect of terminating the thread is that no further messages are sent to its corresponding ball, i.e., it stops moving.




To complete this program, we need a main class which must correspond to the template below:




public class bSim extends GraphicsProgram {




// Parameters used in this program




public void run() {




// Set up display, create and start multiple instances of aBall

}

}




Your main program should set up the display as in Assignment 1.  The screen is laid out as a 1200 x 800 rectangle, with the ground plane sitting at 600 (offset of 200).  You will run your final simulation with 100 balls (although for debugging purposes it is suggested that you use smaller values, e.g., 1).   The aBall constructor needs 7 parameters – Xi, Yi, Vo, theta, bSize, bColor, and bLoss.  Use the RandomGenerator class shown in the slides to generate values for each aBall instance.  For example, to generate a random loss parameter, one would use an instance of the RandomGenerator class as follows:




double iLoss = rgen.nextDouble(EMIN,EMAX);




Finally, to generate a simulation with NUMBALLS elements, one can easily set up a for loop that on each iteration generates a new set of random parameters, creates a aBall instance using these parameters, and starting the corresponding thread.




You might also consider writing a “helper” class with utility functions, e.g. methods for converting from simulation coordinates to screen coordinates, etc.  Although this is not strictly required, it does make your code a lot easier to read and understand.




<h1>Instructions</h1>




<ol>

 <li>Write Java classes, java, aBall.java, and gUtil.java (optionally), that implement the simulation outlined above. If you wish to implement using more classes than these three, you can do so. Do this within the Eclipse environment so that it can be readily tested by the course graders.  For your own benefit, you should get in the habit of naming your Eclipse projects so that they can easily be identified, e.g., ECSE202_A2.</li>

 <li>Edit the parameters of bSim to replicate the output of Assignment 1, i.e., generate a single aBall instance with Xi=5m, Yi=1m, Vo=40 m/s, theta=85°, loss=0.4, radius=1m. Verify that it traces the identical path to the bounce class.  Take a snapshot of the screen when the simulation reaches steady-state and save as A2-1.pdf.</li>

 <li>Repeat Question 2, but this time with parameters Xi=95m, Yi=1m, Vo=40 m/s, theta=95°, loss=0.4, radius=1m. Verify that the path is identical to Question 2, except that the ball moves from right to left.  Take a snapshot of the screen when the simulation reaches steady-state and save as A2-2.pdf.</li>

 <li>Perform a simulation with 100 balls using the parameters listed on Page 2. In order for us to validate your simulation we need a means to ensure that all simulations produce identical results.  After creating a the random number generator object do the following:     setSeed((long) 0.12345);</li>

</ol>

This will produce the same sequence of psuedo random numbers for each program run, terminating at exactly the same state.  Take a snapshot of the screen when the simulation reaches steady-state and save as A2-3.pdf.