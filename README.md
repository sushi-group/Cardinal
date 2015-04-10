# Cardinal
Test cardinal


	import java.awt.AWTException;
import java.awt.Dimension;
//import java.awt.Graphics2D;
import java.awt.Rectangle;
//import java.awt.RenderingHints;
import java.awt.Robot;
import java.awt.Toolkit;
import java.awt.event.InputEvent;
import java.awt.event.KeyEvent;
import java.awt.image.BufferedImage;
import java.awt.image.RasterFormatException;
import java.io.File;
import java.io.IOException;

import javax.imageio.ImageIO;


	public class Cardinal {
		private static int[] gameRegion;
		
		    public Cardinal(){
		     	gameRegion = new int[4];
                		    
		    
		    
		    }
	       
		    public static void leftClick(Robot robot) {
			    robot.mousePress(InputEvent.BUTTON1_MASK);
			    robot.delay(100);
			    robot.mouseRelease(InputEvent.BUTTON1_MASK);
			    robot.delay(100);
			  }  
		    
		    
		    public void navigateStartGameMenu(Robot robot) {
				robot.mouseMove (gameRegion[0]+ 248, gameRegion[1]+ 462);
				leftClick(robot);
			
			} 
		    
		static boolean bufferedImagesEqual(BufferedImage img1, BufferedImage img2) {
		    if (img1.getWidth() == img2.getWidth() && img1.getHeight() == img2.getHeight()) {
		        for (int x = 0; x < img1.getWidth(); x++) {
		            for (int y = 0; y < img1.getHeight(); y++) {
		                if (img1.getRGB(x, y) != img2.getRGB(x, y))
		                    return false;
		            }
		        }
		    } else {
		        return false;
		    }
		    return true;
		}
		
		public void getGameRegion() throws AWTException {
			Robot robot = new Robot();
			Dimension resolution = Toolkit.getDefaultToolkit().getScreenSize();
			Rectangle rectangle = new Rectangle(resolution);
			BufferedImage fullScreen = robot.createScreenCapture(rectangle);
			BufferedImage topRightCorner = new BufferedImage(78,68,BufferedImage.TYPE_INT_ARGB);
			BufferedImage img = new BufferedImage(78,68,BufferedImage.TYPE_INT_ARGB);
			try {
				topRightCorner = ImageIO.read(new File("top2.png"));
			} catch (IOException e) {
			}
			for(int x = 0; x < fullScreen.getWidth(); x++) {
				for(int y = 0; y < fullScreen.getHeight(); y++) {
					if(gameRegion[0] == 0 && gameRegion[1] == 0) {
						try {
							img = fullScreen.getSubimage(x, y, 78, 68);
							if(bufferedImagesEqual(img,topRightCorner)) {
								gameRegion[0] = x - 437;
								gameRegion[1] = y;
							}
						} catch (RasterFormatException r) {
						}
					}
				}
			}
			System.out.println(Cardinal.gameRegion[0] + " " + Cardinal.gameRegion[1]);
			gameRegion[2] = 495;
			gameRegion[3] = 495;
		}
		
		
		 
		   public static BufferedImage getImage() throws IOException, AWTException{
		     
			   Robot robot = new Robot();
				Dimension resolution = Toolkit.getDefaultToolkit().getScreenSize();
				Rectangle rectangle = new Rectangle(resolution);
				BufferedImage fullScreen = robot.createScreenCapture(rectangle);
			    BufferedImage img = new BufferedImage(495,495,BufferedImage.TYPE_INT_ARGB);
			    img = fullScreen.getSubimage(gameRegion[0], gameRegion[1], gameRegion[2], gameRegion[3]);
			
		
			    
			    
				return img;
		   }  
				public void getBouton() throws IOException, AWTException{
					Robot robot = new Robot();
				
				 if(getImage().getRGB( 248 ,68 ) != -5242598) { 
					System.out.println("up");
					clickup(robot);
					
				}
				
				else if (getImage().getRGB( 73    , 251) != -5242598){
					System.out.println("gauche");
					clickgauche(robot);
				
				}
				else if(getImage().getRGB( 248    , 427) != -5242598) {
					System.out.println("down");
					clickdown(robot );
				 }
				else if ( getImage().getRGB( 422    , 251) != -5242598){
					System.out.println("droite");
					clickdroit(robot);
				}
		   
		   
		   }
	
	   
			public static void clickdroit(Robot robot){
				
				robot.keyPress(KeyEvent.VK_RIGHT);
				robot.delay(100);
				robot.keyRelease(KeyEvent.VK_RIGHT);
				robot.delay(100);
			}
			
			public static void clickgauche ( Robot robot){
			   robot.keyPress(KeyEvent.VK_LEFT);
			   robot.delay(100);
				robot.keyRelease(KeyEvent.VK_LEFT);
				robot.delay(100);
			}
		   
			public static void  clickup(Robot robot){
				robot.keyPress(KeyEvent.VK_UP);	
				robot.delay(100);
				robot.keyRelease(KeyEvent.VK_UP);
				robot.delay(100);
			}
			public static void clickdown(Robot robot){
				robot.keyPress(KeyEvent.VK_DOWN);	
				robot.delay(100);
				robot.keyRelease(KeyEvent.VK_DOWN);
			   robot.delay(100);
			}
			public static boolean gameover() throws IOException, AWTException{
				if (getImage().getRGB(255,255)==-1){
					return true ;
					
				}
				return false;
			}
			
			public static void main(String[] args) throws AWTException, IOException, InterruptedException {
		    
			
		   Robot robot = new Robot();
			
			Cardinal c = new Cardinal();
         	c.getGameRegion();
		 c.navigateStartGameMenu(robot);
		  
		  while (gameover()== false){
			
		 	c.getBouton();
		 	Thread.sleep(450);
	     
		  }
		
		
		 /*	Cardinal.screenShot(
					 
					//Les Dimensions de l'ecran que tu veux
					new Rectangle(gameRegion[0], gameRegion[1], 495, 495),
		 
					//Les Dimensions de l'image de l'ecran
					new Dimension(495, 495),
		 
					//format de l'image resultante 
					"test" + i +".png",
		 
					Cardinal.IMAGE_TYPE_PNG);*/
			
		}
					}
		
