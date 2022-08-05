Steps for Color Detection Project

Here are the steps to build an application in Python that can detect colors:
1. Download the files
•	Color_detection.py – main source code of the project.
•	Colorpic.jpg – sample image for experimenting.
•	Colors.csv – a file that contains  dataset.
2. Taking an image from the user
use argparse library to create an argument parser. Then directly give an image path from the command prompt:
import argparse
ap = argparse.ArgumentParser()
ap.add_argument('-i', '--image', required=True, help="Image Path")
args = vars(ap.parse_args())
img_path = args['image']
#Reading image with opencv
img = cv2.imread(img_path)<div class="open_grepper_editor" title="Edit & Save To Grepper"></div>
3. Next,  read the CSV file with pandas
The pandas library is very useful when you need to perform various operations on data files like CSV. pd.read_csv() reads the CSV file and loads it into the pandas DataFrame. you have assigned each column with a name for easy accessing.
#Reading csv file with pandas and giving names to each column
index=["color","color_name","hex","R","G","B"]
csv = pd.read_csv('colors.csv', names=index, header=None)<div class="open_grepper_editor" title="Edit & Save To Grepper"></div>
4. Set a mouse callback event on a window
First, you create a window in which the input image will display. Then, set a callback function which will be called when a mouse event happens.
cv2.namedWindow('image')
cv2.setMouseCallback('image',draw_function)<div class="open_grepper_editor" title="Edit & Save To Grepper"></div>
With these lines,  name the window as ‘image’ and set a callback function which will call the draw_function() whenever a mouse event occurs.

5. Create the draw_function
It will calculate the rgb values of the pixel which you double click. The function parameters have the event name, (x,y) coordinates of the mouse position, etc. In the function,  check if the event is double-clicked then calculate and set the r,g,b values along with x,y positions of the mouse.
def draw_function(event, x,y,flags,param):
if event == cv2.EVENT_LBUTTONDBLCLK:
global b,g,r,xpos,ypos, clicked
clicked = True
xpos = x
ypos = y
b,g,r = img[y,x]
b = int(b)
g = int(g)
r = int(r)<div class="open_grepper_editor" title="Edit & Save To Grepper"></div>
6. Calculate distance to get color name
 have the r,g and b values. Now, you need another function which will return us the color name from RGB values. To get the color name, calculate a distance(d) which tells  how close you are to color and choose the one having minimum distance.
The distance is calculated by this formula:
d = abs(Red – ithRedColor) + (Green – ithGreenColor) + (Blue – ithBlueColor)
def getColorName(R,G,B):
minimum = 10000
for i in range(len(csv)):
d = abs(R- int(csv.loc[i,"R"])) + abs(G- int(csv.loc[i,"G"]))+ abs(B- int(csv.loc[i,"B"]))
if(d<=minimum):
minimum = d
cname = csv.loc[i,"color_name"]
return cname<div class="open_grepper_editor" title="Edit & Save To Grepper"></div>
7. Display image on the window
Whenever a double click event occurs, it will update the color name and RGB values on the window.
Using the cv2.imshow() function, you draw the image on the window. When the user double clicks the window, you draw a rectangle and get the color name to draw text on the window using cv2.rectangle and cv2.putText() functions.
while(1):
cv2.imshow("image",img)
if (clicked):
#cv2.rectangle(image, startpoint, endpoint, color, thickness) -1 thickness fills rectangle entirely
cv2.rectangle(img,(20,20), (750,60), (b,g,r), -1)
#Creating text string to display ( Color name and RGB values )
text = getColorName(r,g,b) + ' R='+ str(r) + ' G='+ str(g) + ' B='+ str(b)
#cv2.putText(img,text,start,font(0-7), fontScale, color, thickness, lineType, (optional bottomLeft bool) )
cv2.putText(img, text,(50,50),2,0.8,(255,255,255),2,cv2.LINE_AA)
#For very light colours you will display text in black colour
if(r+g+b>=600):
cv2.putText(img, text,(50,50),2,0.8,(0,0,0),2,cv2.LINE_AA)
clicked=False
#Break the loop when user hits 'esc' key 
if cv2.waitKey(20) & 0xFF ==27:
break
cv2.destroyAllWindows()<div class="open_grepper_editor" title="Edit & Save To Grepper"></div>
8. Run Python File
Run the Python file from the command prompt. Make sure to give an image path using ‘-i’ argument. If the image is in another directory, then you need to give full path of the image:
python color_detection.py -i <add your image path here><div class="open_grepper_editor" title="Edit & Save To Grepper"></div>








