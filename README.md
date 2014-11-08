PiDay
=====

Processing GIF Animation Generation

1. Download Processing from processing.org
2. Copy-Paste piday script below as a new file, and save it.
3. Once 2 is done, download and extract gifAnimation to the /home/you/sketchbook/libraries/ , which should become existant after saving of the file.

```
#!javascript

import gifAnimation.*;
 
GifMaker gifExport;
int frames = 0;
int totalFrames = 65;
 
int xspacing = 2;   // How far apart should each horizontal location be spaced
int hspacing = 120; // Distance from right side to the head.
int w;              // Width of entire wave
 
float theta = 0.0;  // Start angle at 0
float amplitude = 75.0;  // Height of wave
float period = 500.0;  // How many pixels before the wave repeats
float dx;  // Value for incrementing X, a function of period and xspacing
float[] yvalues;  // Using an array to store height values for the wave
float[] cvalues;  // Extra for plotting cosine wave together
 
 
public void setup() {
  smooth();
  //size(497, 373);
  size(497, 373);
  gifExport = new GifMaker(this, "pi.gif", 100);
  gifExport.setRepeat(0); // make it an "endless" animation
  w = width+16;
  dx = (TWO_PI / period) * xspacing;
  yvalues = new float[w/xspacing];
  cvalues = new float[w/xspacing];
  ellipseMode(CENTER);
}
 
void draw() {
  background(255);
  PFont f;
  f = createFont("Times New Roman",16,true);
  textFont(f,21);
  text("    Because for every x=𝜋k, k ∈ ℕ,",50,30);
  stroke(0,0,255);
  text(" sin(1/4𝜋+x) = cos(1/4𝜋+x) , 𝜋=3.14...",   50,height-30);
  PFont g;
  g = createFont("Times New Roman", 16, true);
  textFont(g,10);
  text("Happy Platonic Valentine's Day ^__^!", width-222, height-10);
  calcWave();
  renderWave();
  export();
}
 
void calcWave() {
  // Increment theta (try different values for 'angular velocity' here
  theta += 0.1;
 
  // For every x value, calculate a y value with sine function
  float x = theta;
  for (int i = 0; i < yvalues.length; i++) {
    yvalues[i] = sin(x)*amplitude;
    cvalues[i] = cos(x)*amplitude;
    x+=dx;
  }
 
}
 
void renderWave() {
  noStroke();
  fill(255);
  // A simple way to draw the wave with an ellipse at each location
  // Draw Tails
  for (int x = 0; x < yvalues.length; x++) {
    for (int z = -6; z < 6; z++) {
      stroke(255,0,0);
      point(x*xspacing-hspacing, height/2+yvalues[x]+z);
      stroke(0,0,255);
      point(x*xspacing-hspacing, height/2+cvalues[x]+z);
    }
  }
  // Draw Heads
  pushMatrix();
  stroke(0,0,255); fill(0,0,255);
  translate(xspacing*(yvalues.length-1)-hspacing, height/2+cvalues[yvalues.length-1]); rotate(-radians(height/2+yvalues[yvalues.length-1]/2));
  ellipse(-50,0, 100, 80);
  popMatrix();
  pushMatrix();
  stroke(255,0,0); fill(255,0,0);
  translate(xspacing*(yvalues.length-1)-hspacing, height/2+yvalues[yvalues.length-1]); rotate(radians(height/2+cvalues[cvalues.length-1]/2));
  ellipse(-50, 0, 100, 80);
  popMatrix();
}
 
void export() {
  if(frames < totalFrames) {
  gifExport.setDelay(20);
  gifExport.addFrame();
  frames++;
  } else {
  gifExport.finish();
  frames++;
  println("gif saved");
  exit();
  }
}
```
