import java.awt.BorderLayout;

import java.awt.Button;
import java.awt.Choice;
import java.awt.Color;
import java.awt.Component;
import java.awt.Dimension;
import java.awt.FlowLayout;
import java.awt.Font;
import java.awt.Frame;
import java.awt.Label;
import java.awt.Panel;
import java.awt.TextArea;
import java.awt.TextField;
import java.awt.Toolkit;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.FocusEvent;
import java.awt.event.FocusListener;
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;
import java.util.ArrayList;

import javax.swing.Box;
import javax.swing.JScrollPane;


//idea: method to draw panel elements, with an array of things to add
//also method to create new panel

public class GUI extends Frame implements WindowListener, ActionListener
{
	
	/**
	 * THIS CLASS RUNS THE ENTIRE PROGRAM, SETS UP THE GUI
	 */
	private static final long serialVersionUID = 1L;
	Panel panel;
	Dimension dim=Toolkit.getDefaultToolkit().getScreenSize();
	private final int LENGTH=1300;
	private final int WIDTH=LENGTH/2;
	private int numPlayers=1;
	private Engine engine;
	private ArrayList<StatLine> statline;
	class StatLine//nested class, to be used later
	{
		String first;
		String last;
		String year1;
		String year2;
		StatLine(String f, String l, String y1,String y2)
		{
			first=f;
			last=l;
			year1=y1;
			year2=y2;
		}
	}
	
	public GUI()//setting up GUI (which is a Frame)
	{
		addWindowListener(this);
		setResizable(false);
		setSize(new Dimension(LENGTH,WIDTH));
		setLocation(dim.width/2-this.getSize().width/2, dim.height/2-this.getSize().height/2);
		setTitle("StatGrabber (Statistics provided by Pro Football Reference)");
		setLayout(new BorderLayout());
		panel=new Panel();
		engine=new Engine();
		statline=new ArrayList<StatLine>();
		add(panel, BorderLayout.CENTER);
		init();
	}
	public void init()//draws panel, initial screen
	{
		panel.removeAll();
		panel.setLayout(new FlowLayout());
		for(int i=0; i<numPlayers; i++)//adds a label, two textfields, two year selectors, and a removal button for all players
		{
			Label namesLabel=new Label("Enter the name of a player: ");
			namesLabel.setFont(new Font("Arial", Font.BOLD, 25));
			
			TextField firstName=new TextField(10);
			firstName= createTextPrompt(firstName, "First");
			
			TextField lastName=new TextField(10);
			lastName=createTextPrompt(lastName, "Last");
			
			Choice year1= new Choice();
			year1=createYearPrompt(year1);
			year1.setFont(new Font("Arial",Font.PLAIN, 25));
			
			Choice year2= new Choice();
			year2=createYearPrompt(year2);
			year2.setFont(new Font("Arial",Font.PLAIN, 25));
			
			Button b= new Button("X");
			setContainerSize(b, 30,30);
			b.setFont(new Font("Arial", Font.BOLD, 20));
			b.addActionListener(new ActionListener()
			{

				@Override
				public void actionPerformed(ActionEvent e)
				{
					numPlayers--;
					init();//redraw panel
				}//removes a row
				
			}
					);
			
			Label dash= new Label("-");
			dash.setFont(new Font("Arial",Font.PLAIN, 30));
			
			panel.add(namesLabel);
			panel.add(firstName);
			panel.add(lastName);
			panel.add(Box.createRigidArea(new Dimension(80, 40)));
			panel.add(year1);
			panel.add(dash);
			panel.add(year2);
			panel.add(b);
		}
		//Only one of each of these elements
		Button addPlayer=new Button("+");
		addPlayer.setFont(new Font("Arial", Font.PLAIN, 30));
		addPlayer.addActionListener(new ActionListener()
		{
			@Override
			public void actionPerformed(ActionEvent e)
			{
				numPlayers++;
				init();//redraw panel, adds a row
			}


		}
		);
		setContainerSize(addPlayer, 250, 40);
	
		panel.add(addPlayer, Panel.LEFT_ALIGNMENT);
		panel.add(Box.createRigidArea(new Dimension(400,40)));
		panel.add(Box.createRigidArea(new Dimension(1000,40)));
		
		Label label= new Label("Stastic: ");
		label.setFont(new Font("Arial", Font.BOLD, 25));
		
		Choice stat= new Choice();
		stat=addStats(stat);
		stat.setFont(new Font("Arial", Font.PLAIN, 28));
		setContainerSize(stat,500,40);
		panel.add(stat);
		
		Choice c= new Choice();
		c.add("Cumulative");
		c.add("By Year");
		c.setFont(new Font("Arial", Font.PLAIN,28));
		setContainerSize(c,200,40);
		panel.add(c);
		panel.add(Box.createRigidArea(new Dimension(50,40)));
		
		Button submit=new Button("Find statistics");
		submit.setFont(new Font("Arial", Font.BOLD, 30));
		submit.addActionListener(this);//when the "submit" button is pressed, the method GUI.actionPerformed() is triggered
		panel.add(submit);
		setVisible(true);
		
	}
	private Choice addStats(Choice stat)//adds these choices to the "choose statistic" list
	{
		stat.add("Passing Yards");
		stat.add("Passing Touchdowns");
		stat.add("Completion Percentage");
		stat.add("Interceptions Thrown");
		stat.add("Times Sacked");
		stat.add("Record");
		stat.add("QBR");
		stat.add("Passer Rating");
		stat.add("Yards per Completion");
		
		stat.add("Rushing Yards");
		stat.add("Receiving Yards");
		stat.add("Rushing Touchdowns");
		stat.add("Receiving Touchdowns");
		stat.add("Fumbles");
		stat.add("Yards from Scrimmage");
		stat.add("Yards per Rushing Attempt");
		stat.add("Yards per Catch");
		stat.add("Rushing and Receiving Touchdowns");
		
		stat.add("Interceptions");
		stat.add("Forced Fumbles");
		stat.add("Fumbles Recovered");
		stat.add("Sacks");
		stat.add("Solo Tackles");
		stat.add("Interception Touchdowns");
		stat.add("Fumble Recovery Touchdowns");
		stat.add("Passes Defended");
		
		return stat;
	}
	
	private void setContainerSize(Component comp, int length, int width)//sets this container at a set length and width
	{
		comp.setPreferredSize(new Dimension(length,width));
		comp.setMinimumSize(new Dimension(length,width));
		comp.setMaximumSize(new Dimension(length,width));
	}
	
	private TextField createTextPrompt(TextField t, String initial)//creates custom textfield
	{
		t.setText(initial);
		setContainerSize(t, 200, 40);
		t.setFont(new Font("Arial", Font.PLAIN, 30));
		t.setForeground(Color.BLACK);
		t.addFocusListener(new FocusListener(){
		public void focusGained(FocusEvent event){if (t.getText().equals(initial))t.setText("");}

		@Override
		public void focusLost(FocusEvent arg0)
		{
			if(t.getText().equals(""))t.setText(initial);
		}});//when highlighted, makes initial text disappear and vice versa
		return t;
	}
	
	private Choice createYearPrompt(Choice c)//year choices from 2014 (most recent season) to 1920
	{
		setContainerSize(c,100,25);
		for(Integer i=2014; i>=1920; i--)
		{
			c.add(i.toString());
		}
		return c;
	}
	
	public void actionPerformed(ActionEvent e)//submit button is pressed
	{
		statline=new ArrayList<StatLine>();
		Component [] ca= panel.getComponents();		
		for(int i=0; i<panel.getComponents().length; i++)
		{
			if(i%8==1)//if second element in row
			{
				if(ca[i] instanceof TextField)//if "First Name" textfield
				{
					TextField f= (TextField)ca[i];//firstname
					TextField l=(TextField)ca[i+1];//lastname
					Choice y1=(Choice)ca[i+3];//start year
					Choice y2=(Choice)ca[i+5];//end year
					StatLine s= new StatLine(f.getText(),l.getText(),y1.getSelectedItem(), y2.getSelectedItem());
					statline.add(s);//list of information used to obtain the statistics, taken from user-entered data
				}
			}
		}
		display();
	}
	
	public void display()
	{
		Object[][] o= new Object[statline.size()][5];
		Component[] c= panel.getComponents();
		if(!(c[c.length-1] instanceof Button))panel.remove(c.length-1);//if there is an "error" label, remove it
		c=panel.getComponents();
		Choice ch=(Choice) c[c.length-3];
		String choice= ch.getSelectedItem();//cumulative or by year, aka type
		Choice st=(Choice)c[c.length-4];
		String stat=st.getSelectedItem();//desired statistic
		for(int i=0; i<statline.size(); i++)
		{
			StatLine s=statline.get(i);
			try
			{
				System.out.println(s.first+" "+s.last+" "+stat+" "+s.year1+" "+s.year2+" "+choice);//bug checking
				o[i]=engine.getStatistic(s.first, s.last, stat, s.year1, s.year2, choice);//gets the actual data from each "statline" (user-entered data)
				System.out.println(o[i][1]);//more bug checking
			} 
			catch (Exception e)
			{
				e.printStackTrace();
			}
		}
		ArrayList<TextArea> textareas=new ArrayList<TextArea>();
		ArrayList<JScrollPane> scrollpanes= new ArrayList<JScrollPane>();
		int count=-1;
		for(int i=0; i<o.length; i++)//iterate through each player, create textareas for each player
		{
			if(o[i].length!=4)//if there is an error message
			{
				count=i;
				break;
			}
			TextArea ta= new TextArea();
			ta.setFont(new Font("Arial",Font.PLAIN,20));
			ta.append(o[i][2]+"\n");//first, last, name of player
			Object[] somestats=(Object[])o[i][0];//the "result" array from getStatistic
			ta.append("Age(s): "+ somestats[0]+"\n"+ "Position: "+somestats[2]+"\n"+"Team(s): "+somestats[1]+"\n");
			ta.append("Games Played: "+somestats[3]+" \n"+"Statistic: "+o[i][3]+"\n");
			String result= (String)o[i][1];
			ta.append(result);
			setContainerSize(ta, 300, 200);
			JScrollPane scrollPane= new JScrollPane(ta);
			textareas.add(ta);
			scrollpanes.add(scrollPane);
		}
		if (count!=-1)//create a label explaining the error message
		{
			String s= (String)o[count][1];
			Label label= new Label(s);
			label.setFont(new Font("Arial", Font.BOLD, 25));
			label.setForeground(Color.RED);
			panel.add(label);
			panel.revalidate();
			validate();
		}
		else
		{
		panel.removeAll();//new screen
		for(int i=0; i<textareas.size(); i++)//no errors, so go ahead and add all the player's textareas
		{
			System.out.println("ADDING");//bug checking
			TextArea textarea=textareas.get(i);
			textarea.setEditable(false);
			panel.add(textarea);
			JScrollPane sp=scrollpanes.get(i);
			panel.add(sp);
		}
		
		panel.add(Box.createRigidArea(new Dimension(LENGTH, 40)));
		Button button=new Button("Return");
		setContainerSize(button, 300,40);
		button.setFont(new Font("Arial", Font.PLAIN, 30));
		button.addActionListener(new ActionListener()
		{
			@Override
			public void actionPerformed(ActionEvent arg0)
			{
				numPlayers=1;
				init();
			}//when pressed, resets the screen to original		
		});
		panel.add(button, Panel.CENTER_ALIGNMENT);
		panel.revalidate();
		validate();
		}
	}
	
	public static void main(String[] args)
	{
		new GUI();
	}//initializes GUI


	@Override
	public void windowClosing(WindowEvent arg0)
	{
		System.exit(0);
	}//when window closes, close the program
	@Override
	public void windowClosed(WindowEvent arg0){}
	@Override
	public void windowActivated(WindowEvent arg0){}
	@Override
	public void windowDeactivated(WindowEvent arg0){}
	@Override
	public void windowDeiconified(WindowEvent arg0){}
	@Override
	public void windowIconified(WindowEvent arg0){}
	@Override
	public void windowOpened(WindowEvent arg0){	}
}
