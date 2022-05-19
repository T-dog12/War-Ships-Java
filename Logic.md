package Game;

import java.awt.*;
import java.awt.event.*;
import java.util.*;
import javax.swing.*;

public class WarShips implements ActionListener{

	Random ran = new Random();
	
	JFrame frame = new JFrame();
	
	JPanel titlePanel = new JPanel();
	JPanel buttonPanel = new JPanel();
	JLabel textfield = new JLabel();
	
	JButton[] buttons = new JButton[8*8];
	
	int[][] points = {{0,0,0,0,0,0,0,0}, {0,0,0,0,0,0,0,0} ,{0,0,0,0,0,0,0,0}, {0,0,0,0,0,0,0,0},
			{0,0,0,0,0,0,0,0}, {0,0,0,0,0,0,0,0}, {0,0,0,0,0,0,0,0}, {0,0,0,0,0,0,0,0}};
	int turns = 0;
	
	WarShips() {
		generate();
		
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		frame.setSize(800,800);
		frame.setLocationRelativeTo(null);
		frame.getContentPane().setBackground(new Color(50,50,50));
		frame.setLayout(new BorderLayout());
		frame.setVisible(true);
		
		textfield.setBackground(new Color(25,25,25));
		textfield.setForeground(new Color(25,255,0));
		textfield.setFont(new Font("Ink Free", Font.BOLD,75));

		textfield.setHorizontalAlignment(JLabel.CENTER);
		textfield.setText("War Ships");
		textfield.setOpaque(true);
		
		titlePanel.setLayout(new BorderLayout());
		titlePanel.setBounds(0,0,800,100);
		
		buttonPanel.setLayout(new GridLayout(8,8));
		buttonPanel.setBackground(new Color(150,150,150));
		
		for(int x = 0; x < 8*8; x++) {
			buttons[x] = new JButton();
			buttonPanel.add(buttons[x]);
			
			buttons[x].setFont(new Font("MV boli", Font.BOLD,120));
			buttons[x].setBackground(new Color(192,192,192));
			buttons[x].setFocusable(false);
			buttons[x].addActionListener(this);
		}
		
		titlePanel.add(textfield);
		frame.add(titlePanel,BorderLayout.NORTH);
		frame.add(buttonPanel);
		
	}
	
	public void generate() {
		for (int z = 0; z < 10;z++) {
			int x = ran.nextInt(8);
			int y = ran.nextInt(8);
			
			while (points[y][x] == 1) {
				x = ran.nextInt(8);
				y = ran.nextInt(8);
			}
			
			points[y][x] = 1;
			
		}
	}
	
	public void check() {
		boolean win = true;
		
		if (turns == 40) {
			for (int z= 0;z< 8*8; z++) {
				buttons[z].setEnabled(false);
			}
			textfield.setText("You Lose!");
		}
		
		for (int x= 0; x < 8; x++) {
			for (int y= 0; y < 8; y++) {
				if(points[x][y] == 1) {
					return;
				}else {
					win = true;
				}
			}
		}
		
		if (win) {
			for (int z= 0;z< 8*8; z++) {
				buttons[z].setEnabled(false);
			}
			textfield.setText("You Win!");
		}
		
	}
	
	@Override
	public void actionPerformed(ActionEvent e) {
		for(int i = 0; i <8*8; i++) {
			if (e.getSource()== buttons[i]) {
				turns += 1;
				
				int x= i % 8;
				int y= i / 8;
				
				if (points[y][x] == 1) {
					buttons[i].setBackground(Color.green);
					
					points[y][x] = 0;
					
					buttons[i].setEnabled(false);
					
					
				}else {
					buttons[i].setBackground(Color.red);
					buttons[i].setEnabled(false);
				}
				
				check();
			}
		}
	}

}
