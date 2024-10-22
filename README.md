# 2023011775-
package middletest;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;

public class Calculator extends JFrame implements ActionListener, KeyListener {
    private JTextField display;
    private String operator;
    private double firstNumber;

    public Calculator() {
        // 프레임 설정
        setTitle("계산기");
        setSize(300, 400);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        // 디스플레이 설정
        display = new JTextField();
        display.setEditable(false);
        display.setHorizontalAlignment(SwingConstants.RIGHT);
        display.setBackground(Color.WHITE); // 배경을 흰색으로 설정
        display.setPreferredSize(new Dimension(300, 65)); // 텍스트 필드 크기 조정 (높이 65)
        add(display, BorderLayout.NORTH);

        // 버튼 패널 설정
        JPanel buttonPanel = new JPanel();
        buttonPanel.setLayout(new GridLayout(4, 4));

        // 버튼 생성
        String[] buttonLabels = {
            "7", "8", "9", "/",
            "4", "5", "6", "*",
            "1", "2", "3", "-",
            "0", "C", "=", "+"
        };

        for (String label : buttonLabels) {
            JButton button = new JButton(label);
            button.setBackground(Color.BLACK); // 버튼 배경을 검은색으로 설정
            button.setForeground(Color.WHITE); // 버튼 텍스트 색상을 흰색으로 설정
            button.addActionListener(this);
            buttonPanel.add(button);
        }

        add(buttonPanel, BorderLayout.CENTER);

        // 키 리스너 추가
        display.addKeyListener(this);
        setFocusable(true); // 프레임이 키 입력을 받을 수 있도록 설정
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        String command = e.getActionCommand();
        processCommand(command);
    }

    @Override
    public void keyTyped(KeyEvent e) {
        char keyChar = e.getKeyChar();
        if (Character.isDigit(keyChar) || keyChar == 'C' || keyChar == '=' || 
            keyChar == '+' || keyChar == '-' || keyChar == '*' || keyChar == '/') {
            processCommand(String.valueOf(keyChar));
        } else if (keyChar == KeyEvent.VK_BACK_SPACE) {
            // 백스페이스 키 처리
            if (display.getText().length() > 0) {
                display.setText(display.getText().substring(0, display.getText().length() - 1));
            }
        } else if (keyChar == KeyEvent.VK_ENTER) {
            // Enter 키 처리
            processCommand("=");
        }
    }

    @Override
    public void keyPressed(KeyEvent e) {
        // 필요하지 않음
    }

    @Override
    public void keyReleased(KeyEvent e) {
        // 필요하지 않음
    }

    private void processCommand(String command) {
        if (command.charAt(0) >= '0' && command.charAt(0) <= '9') {
            display.setText(display.getText() + command);
        } else if (command.equals("C")) {
            display.setText("");
            firstNumber = 0;
            operator = null;
        } else if (command.equals("=")) {
            double secondNumber = Double.parseDouble(display.getText());
            double result = 0;

            switch (operator) {
                case "+":
                    result = firstNumber + secondNumber;
                    break;
                case "-":
                    result = firstNumber - secondNumber;
                    break;
                case "*":
                    result = firstNumber * secondNumber;
                    break;
                case "/":
                    if (secondNumber != 0) {
                        result = firstNumber / secondNumber;
                    } else {
                        display.setText("Error");
                        return;
                    }
                    break;
            }

            // 결과에서 .0 제거
            if (result == (int) result) {
                display.setText(String.valueOf((int) result));
            } else {
                display.setText(String.valueOf(result));
            }
            operator = null;
        } else {
            firstNumber = Double.parseDouble(display.getText());
            operator = command;
            display.setText("");
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            Calculator calculator = new Calculator();
            calculator.setVisible(true);
        });
    }
}
@뤼튼ai
