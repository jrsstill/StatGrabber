import java.net.*;

import java.util.ArrayList;
import java.io.*;

import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;
import com.google.gson.Gson;

public class Engine
{  
	/* THIS CLASS RETRIEVES THE DATA FROM PRO-FOOTBALL-REFERENCE
	 * EXTERNAL LIBRARIES USED: Gson, JSoup
	 * */
	//idea: method for adding up statistics (based on type)- clean up
	//maybe search playernames first, get min/max year range, then search stats
   public static Object[] getStatistic(String firstName, String lastName, String stat, String sYear, String eYear, String type) throws Exception
   {
	   String category=getCategory(stat);
	   String statistic=stat;
	   stat=getCode(stat);
	   String playerPage=findPlayerPage(firstName, lastName);//url of player page
	   if(stat=="")return new Object[]{null,"THAT IS NOT A VALID STATISTIC"};//the statistic given does not exist
	   if(type=="Cumulative"&&!statistic.equals("QBR")&&!statistic.equals("Passer Rating")&&!stat.equals("rush_yds_per_att")&&!stat.equals("rec_yds_per_rec")&&!stat.equals("pass_yds_per_comp")&&!stat.equals("pass_cmp_perc"))
	   {
		   int y1=Integer.parseInt(sYear);
		   int y2=Integer.parseInt(eYear);
		   if(y1>y2)return new Object[]{null, "YEAR 1 IS GREATER THAN YEAR 2"};
		   Float result=(float) 0;
		   Integer totalgames=0;
		   String maxAge="";
		   String minAge="";
		   String[] results=new String[4];
		   String pos="";
		   ArrayList<String> teams= new ArrayList<String>();
		   int wins=0;
		   int losses=0;
		   int ties=0;
		   String name="";
		   for(Integer i=y1; i<=y2; i++)//add each year together
		   {
			      String[] arr= findStatistic(playerPage, category, stat, i.toString());
				   if(statistic.equals("Record")&&arr[4].length()<10)//adds wins, losses, ties for cumulative record
				   {
					   int w=0;
					   int l=0;
					   int t=0;
				   		for(int k=0; k<arr[4].length();k++)
				   		{
				   			if(arr[4].equals("")){w+=0; l+=0;break;}
				   		else if(arr[4].charAt(k)=='-')
				   		{
				   			for(int j=k+1; j<arr[4].length();j++)
				   			{
				   				if(arr[4].charAt(j)=='-')
				   				{
				   					w=Integer.parseInt(arr[4].substring(0,k));
				   					l=Integer.parseInt(arr[4].substring(k+1,j));
				   					t=Integer.parseInt(arr[4].substring(j+1));
				   					break;
				   				}
				   			}
				   			break;
				   		}
				   		}
				   		wins+=w;losses+=l;ties+=t;
				   }
			      //arr[4]= the result, whether that be an error message, a value, or an empty value (0)
				  else if(arr[4].equals(""))result+=0;
			      else if(arr[4].length()>10)return new Object[]{null,arr[4]};//returns the error message
			      else {result+=Float.parseFloat(arr[4]);}
		          Integer games=Integer.parseInt(arr[3]);
		          totalgames+=games;
		          if(i==y1){minAge=arr[0]; pos=arr[2]; name=arr[5];};
		          if(i==y2)maxAge=arr[0];
		      
		          if(!teams.contains(arr[1]))teams.add(arr[1]);
				   
		   }
		   if(y1==y2)results[0]=minAge;
		   else
		   {
		   results[0]=minAge+"-"+maxAge;
		   }
		   results[2]=pos.toUpperCase();
		   results[1]=teams.toString().replace("[","").replace("]","");
		   results[3]=totalgames.toString();
		   String resultstring="";
		   if(statistic.equals("Record"))resultstring=wins+"-"+losses+"-"+ties;
		   else resultstring=result.toString();
		   if(y1==y2)return new Object[] {results, y1+":  "+resultstring, name,statistic};
		   return new Object[] {results, y1+"-"+y2+":  "+resultstring, name,statistic};//returns an array with all player info needed to display
	   }
	   //else if (type=="By Year") or the statistic is some percentage
	   
		   Integer y1=Integer.parseInt(sYear);
		   int y2=Integer.parseInt(eYear);
		   if(y1>y2)return new Object[]{null, "YEAR 1 IS GREATER THAN YEAR 2"};
		   String[] results=new String[4];
		   Integer totalgames=0;
		   String maxAge="";
		   String minAge="";
		   String result="";
		   String pos="";
		   String name="";
		   ArrayList<String> teams=new ArrayList<String>();
		   for(Integer i=y1; i<=y2; i++)//add each year individually
		   {
			      String[] arr= findStatistic(playerPage, category, stat, i.toString());
			      if(arr[4].equals("")){result+=i+":  N/A"+" \n";continue;}
			      if(arr[4].length()>10)return new Object[] {null, arr[4]};
		          result+=i+":  "+arr[4]+"  \n";
		          Integer games=Integer.parseInt(arr[3]);
		          totalgames+=games;
		          if(i==y1){minAge=arr[0]; pos=arr[2]; name=arr[5];};
		          if(i==y2)maxAge=arr[0];
		          if(!teams.contains(arr[1]))teams.add(arr[1]);
		   }
		   if(y1==y2)results[0]=minAge;
		   else
		   {
		   results[0]=minAge+"-"+maxAge;
		   }
		   results[2]=pos.toUpperCase();
		   results[1]=teams.toString().replace("[","").replace("]","");
		   results[3]=totalgames.toString();
		   return new Object[]{results, result, name,statistic};//returns the same array as cumulative, but the result is broken up into lines instead of being added
	   
      
   }
   
   public static String getCategory(String stat)//Table name of statistic
   {
	   
	   if(stat.equals("Passing Yards"))return "passing";
	   if(stat.equals("Completion Percentage"))return "passing";
	   if(stat.equals("Interceptions Thrown"))return "passing";
	   if(stat.equals("Passing Touchdowns"))return "passing";
	   if(stat.equals("Times Sacked"))return "passing";
	   if(stat.equals("Record"))return "passing";
	   if(stat.equals("QBR"))return "passing";
	   if(stat.equals("Passer Rating"))return "passing";
	   if(stat.equals("Yards per Completion"))return "passing";	  
	   
	   if(stat.equals("Rushing Yards"))return "rushing_and_receiving";
	   if(stat.equals("Receiving Yards"))return "rushing_and_receiving";
	   if(stat.equals("Rushing Touchdowns"))return "rushing_and_receiving";
	   if(stat.equals("Receiving Touchdowns"))return "rushing_and_receiving";
	   if(stat.equals("Fumbles"))return "rushing_and_receiving";
	   if(stat.equals("Yards from Scrimmage"))return "rushing_and_receiving";
	   if(stat.equals("Rushing and Receiving Touchdowns"))return "rushing_and_receiving";
	   if(stat.equals("Yards per Rushing Attempt"))return "rushing_and_receiving";
	   if(stat.equals("Yards per Catch"))return "rushing_and_receiving";
	   
	   if(stat.equals("Interceptions"))return "defense";
	   if(stat.equals("Forced Fumbles"))return "defense";
	   if(stat.equals("Fumbles Recovered"))return "defense";
	   if(stat.equals("Sacks"))return "defense";
	   if(stat.equals("Solo Tackles"))return "defense";
	   if(stat.equals("Interception Touchdowns"))return "defense";
	   if(stat.equals("Fumble Recovery Touchdowns"))return "defense";	   
	   if(stat.equals("Passes Defended"))return "defense";
	   
	   return "";
   }
   
   private static String getCode(String stat)//HTML attribute "data-stat" value
   {
	   if(stat.equals("Passing Yards"))return "pass_yds";
	   if(stat.equals("Completion Percentage"))return "pass_cmp_perc";
	   if(stat.equals("Interceptions Thrown"))return "pass_int";
	   if(stat.equals("Passing Touchdowns"))return "pass_td";
	   if(stat.equals("Times Sacked"))return "pass_sacked";
	   if(stat.equals("Record"))return "qb_rec";
	   if(stat.equals("QBR"))return "qbr";
	   if(stat.equals("Passer Rating"))return "pass_rating";
	   if(stat.equals("Yards per Completion"))return "pass_yds_per_cmp";	   
	   
	   if(stat.equals("Rushing Yards"))return "rush_yds";
	   if(stat.equals("Receiving Yards"))return "rec_yds";
	   if(stat.equals("Rushing Touchdowns"))return "rush_td";
	   if(stat.equals("Receiving Touchdowns"))return "rec_td";
	   if(stat.equals("Fumbles"))return "fumbles";
	   if(stat.equals("Yards from Scrimmage"))return "yds_from_scrimmage";
	   if(stat.equals("Rushing and Receiving Touchdowns"))return "rush_receive_td";
	   if(stat.equals("Yards per Rushing Attempt"))return "rush_yds_per_att";
	   if(stat.equals("Yards per Catch"))return "rec_yds_per_rec";
	   
	   if(stat.equals("Interceptions"))return "def_int";
	   if(stat.equals("Forced Fumbles"))return "fumbles_forced";
	   if(stat.equals("Fumbles Recovered"))return "fumbles_rec";
	   if(stat.equals("Sacks"))return "sacks";
	   if(stat.equals("Solo Tackles"))return "tackles_solo";
	   if(stat.equals("Interception Touchdowns"))return "def_int_td";
	   if(stat.equals("Fumble Recovery Touchdowns"))return "fumbles_rec_td";	
	   if(stat.equals("Passes Defended"))return "pass_defended";
	   
	   return "";
   }
   public static String findPlayerPage(String first, String last) throws Exception
   {
    String google = "http://ajax.googleapis.com/ajax/services/search/web?v=1.0&q=";
    String search = "pro football reference "+first+" "+last;
    String charset = "UTF-8";

    URL url = new URL(google + URLEncoder.encode(search, charset));
    URLConnection conn=url.openConnection();
    conn.setConnectTimeout(5000);
    conn.setReadTimeout(5000);
    Reader reader = new InputStreamReader(conn.getInputStream(), charset);
    GoogleResults results = new Gson().fromJson(reader, GoogleResults.class);
    // Title and url of first result of search
    String url2=results.getResponseData().getResults().get(0).getUrl();
    if(url2.indexOf("pro-football-reference.com")<0)return "X";//not a real football player
    return url2;//url of player page on pro-football-reference
   }
   
   public static String[] findStatistic(String playerPage, String category, String stat, String year) throws Exception
   {
	   if(playerPage.equals("X"))
	   {
	   return new String[] {null,null,null,null,"THAT IS NOT A VALID PLAYER",null};//array will be: age, team, position, games played, result, name
	   }
	   int count=0;
	   int count2=0;
        	Document doc=Jsoup.connect(playerPage).get();
        	Element infobox= doc.getElementById("info_box");
        	Elements infochildren=infobox.children();
        	String name="";
        	for(Element e: infochildren)
        	{
        		if(e.tagName().equals("div")&&e.attr("class").equals("float_left"))
        		{
        			name=e.child(0).text();
        			break;
        		}
        	}//gets the proper name of the player, not just the search
        	Elements tableElements=doc.getElementsByTag("table");//all tables in the html
        	int x=0;
        	if(category.equals("rushing_and_receiving")||category.equals("receiving_and_rushing")||category.equals("defense"))x=1;
        	for(int i=0; i<tableElements.size(); i++)//index of table in tablelist
        	{
        		if(tableElements.get(i).id().equals(category))
        		{
        			boolean broken=false;
        			Element tableh=tableElements.get(i).child(1);
        			for(int j=0; j<tableh.child(x).children().size(); j++)//column of statistic
        			{
        				if(tableh.child(x).child(j).attr("data-stat").equals(stat)){count2=j; broken=true; break;}//count2=column of statstic in table
        			}
        			if(broken){count=i;break;}//count=index of table in table elements
        			else
        			{
        				return new String[] {null,null,null,null,"THAT IS NOT A VALID STATISTIC",null};//stat does not exist in table
        			}
        			
        		}
        		if(i==tableElements.size()-1)//if the table does not exist
        		{
        			if(category.equals("rushing_and_receiving"))return findStatistic(playerPage, "receiving_and_rushing", stat, year);
        			return new String[] {null,null,null,null, "THAT IS NOT A VALID STATISTIC",null};
        		}
        		
        	}
        	String[] result={null,null,null,null,"",null};
            Element tbody=tableElements.get(count).child(2);
        	for(int i=0; i<tbody.children().size(); i++)//iterate through each year
        	{
        		if(tbody.child(i).id().indexOf(year)>=0)
        		{
        			result[0]=tbody.child(i).child(1).text();//age
        			result[1]=tbody.child(i).child(2).text();//team
        			result[2]=tbody.child(i).child(3).text();//position
        			result[3]=tbody.child(i).child(5).text();//games played
        			result[4]= tbody.child(i).child(count2).text();//desired statistic
        			result[5]=name;
        			break;
        		}
        		if(i==tbody.children().size()-1)return new String[] {null, null, null, null, "THAT IS NOT A VALID YEAR",name};//year does not exist in table
        	}
        
        return result;
        //RETURNS AN OBJECT ARRAY OF {AGE, TEAM, POSITION, GAMES PLAYED, RESULT, NAME}
    }
}
    
    

