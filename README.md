# Master-App
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.io.*;
import java.util.Calendar;
import java.util.Date;
import java.text.SimpleDateFormat;
import java.awt.geom.*;

public class SaraswatiMasterApp extends JFrame {
    
    // --- LIGHT & PROFESSIONAL THEME COLORS ---
    Color BG_WHITE = new Color(255, 255, 255);
    Color SIDEBAR_LIGHT = new Color(240, 245, 250);
    Color ACCENT_BLUE = new Color(0, 120, 215);
    Color TEXT_DARK = new Color(30, 30, 30);
    Color SUCCESS_GREEN = new Color(34, 139, 34);

    // --- FONTS ---
    Font typingFont = new Font("Segoe Print", Font.BOLD, 34); 
    Font quoteFont = new Font("Lucida Handwriting", Font.ITALIC, 22);
    Font emojiFont = new Font("Segoe UI Emoji", Font.PLAIN, 16);
    Font boldUI = new Font("Segoe UI", Font.BOLD, 14);

    JPanel cardPanel;
    CardLayout cardLayout;
    JProgressBar chandigarhMeter;
    JLabel welcomeLabel;
    JTextArea quoteArea;
    
    private static final String PIN = "2002";
    private String welcomeText;
    private String motivationText = "“Believe in yourself every day. ✨ Success will follow your effort. 🚀🏃‍♂️”";

    String[] didiMessages = {
        "Oye Chhote! 👦 Tension mat le, tera result bohot solid aayega. ✨",
        "Pulkit, thoda paani pee aur relax kar. Sab theek ho jayega! 💧",
        "Saraswati Traders ka owner itna udaas achha nahi lagta. Smile kar! 😊",
        "BCA ho ya SSC, tu phod dega! Bas koshish mat chhodna. 🚀",
        "Didi hamesha tere saath hai, tension ko kardo 'System Close'! 🔒"
    };

    public SaraswatiMasterApp() {
        // App settings
        setTitle("Saraswati Master Assistant - Pulkit Kumar");
        setSize(1150, 800);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        // Pehle Login Screen dikhao
        showLoginScreen();
    }

    // ==========================================
    // PREMIUM LOGIN INTERFACE (LATEST ADDITION)
    // ==========================================
    private void showLoginScreen() {
        JDialog loginDialog = new JDialog(this, "Security Access", true);
        loginDialog.setSize(450, 500);
        loginDialog.setLocationRelativeTo(null);
        loginDialog.setUndecorated(true); // Removes windows borders

        JPanel mainPanel = new JPanel(new BorderLayout()) {
            @Override
            protected void paintComponent(Graphics g) {
                Graphics2D g2d = (Graphics2D) g;
                GradientPaint gp = new GradientPaint(0, 0, new Color(235, 245, 255), 0, getHeight(), Color.WHITE);
                g2d.setPaint(gp);
                g2d.fillRect(0, 0, getWidth(), getHeight());
                g2d.setColor(ACCENT_BLUE);
                g2d.drawRect(0, 0, getWidth()-1, getHeight()-1);
            }
        };

        JPanel content = new JPanel();
        content.setLayout(new BoxLayout(content, BoxLayout.Y_AXIS));
        content.setOpaque(false);
        content.setBorder(BorderFactory.createEmptyBorder(40, 40, 40, 40));

        JLabel iconLabel = new JLabel("🔐", SwingConstants.CENTER);
        iconLabel.setFont(new Font("Segoe UI Emoji", Font.PLAIN, 50));
        iconLabel.setAlignmentX(Component.CENTER_ALIGNMENT);

        JLabel welcome = new JLabel("Welcome Back, Pulkit");
        welcome.setFont(new Font("Segoe Print", Font.BOLD, 22));
        welcome.setAlignmentX(Component.CENTER_ALIGNMENT);

        JLabel subText = new JLabel("Please enter your Master PIN to unlock");
        subText.setFont(new Font("Segoe UI", Font.PLAIN, 12));
        subText.setForeground(Color.GRAY);
        subText.setAlignmentX(Component.CENTER_ALIGNMENT);

        JPasswordField pinField = new JPasswordField();
        pinField.setMaximumSize(new Dimension(250, 45));
        pinField.setFont(new Font("Arial", Font.BOLD, 24));
        pinField.setHorizontalAlignment(JTextField.CENTER);
        pinField.setBorder(BorderFactory.createMatteBorder(0, 0, 2, 0, ACCENT_BLUE));

        JButton loginBtn = new JButton("Unlock System 🔓");
        loginBtn.setMaximumSize(new Dimension(250, 50));
        loginBtn.setFont(new Font("Segoe UI", Font.BOLD, 14));
        loginBtn.setBackground(ACCENT_BLUE);
        loginBtn.setForeground(Color.WHITE);
        loginBtn.setFocusPainted(false);
        loginBtn.setAlignmentX(Component.CENTER_ALIGNMENT);

        loginBtn.addActionListener(e -> {
            if (new String(pinField.getPassword()).equals(PIN)) {
                loginDialog.dispose();
                initMainDashboard(); // Dashboard setup
                setVisible(true);
            } else {
                JOptionPane.showMessageDialog(loginDialog, "Invalid PIN! ❌", "Access Denied", JOptionPane.ERROR_MESSAGE);
            }
        });

        content.add(iconLabel);
        content.add(Box.createVerticalStrut(20));
        content.add(welcome);
        content.add(subText);
        content.add(Box.createVerticalStrut(40));
        content.add(pinField);
        content.add(Box.createVerticalStrut(30));
        content.add(loginBtn);

        mainPanel.add(content, BorderLayout.CENTER);
        loginDialog.add(mainPanel);
        loginDialog.setVisible(true);
    }

    // ==========================================
    // MAIN DASHBOARD INITIALIZATION
    // ==========================================
    private void initMainDashboard() {
        // --- SIDEBAR DESIGN ---
        JPanel sidebar = new JPanel();
        sidebar.setLayout(new BoxLayout(sidebar, BoxLayout.Y_AXIS));
        sidebar.setBackground(SIDEBAR_LIGHT);
        sidebar.setPreferredSize(new Dimension(250, 800));
        sidebar.setBorder(BorderFactory.createMatteBorder(0, 0, 0, 1, new Color(220, 220, 220)));

        JLabel logo = new JLabel("  📊 ST MANAGER ");
        logo.setFont(new Font("Segoe UI", Font.BOLD, 22));
        logo.setForeground(ACCENT_BLUE);
        logo.setBorder(BorderFactory.createEmptyBorder(30, 10, 50, 10));
        sidebar.add(logo);

        // --- NAV ITEMS ---
        String[] navItems = {"🏠 Dashboard", "🎓 Study Zone", "📱 Shop Billing", "🛠 Repairing", "📝 Personal", "🔍 Records History", "🤖 AI Chatbot"};
        for (String item : navItems) {
            sidebar.add(createNavButton(item));
        }

        // --- MAIN CONTENT AREA ---
        cardLayout = new CardLayout();
        cardPanel = new JPanel(cardLayout);

        cardPanel.add(createDashboard(), "🏠 Dashboard");
        cardPanel.add(createStudyPage(), "🎓 Study Zone");
        cardPanel.add(createShopPage(), "📱 Shop Billing");
        cardPanel.add(createRepairPage(), "🛠 Repairing");
        cardPanel.add(createPersonalPage(), "📝 Personal");
        cardPanel.add(createHistoryPage(), "🔍 Records History");
        cardPanel.add(createChatbotPage(), "🤖 AI Chatbot");

        add(sidebar, BorderLayout.WEST);
        add(cardPanel, BorderLayout.CENTER);

        // Start Animation
        startTypingAnimation();
    } // <--- Ye bracket check karo, ye band hona zaroori hai!
    private JButton createNavButton(String text) {
        JButton btn = new JButton(text);
       btn.setMaximumSize(new Dimension(250, 55));
        btn.setFont(emojiFont);
        btn.setForeground(TEXT_DARK);
        btn.setBackground(SIDEBAR_LIGHT);
        btn.setFocusPainted(false);
        btn.setBorder(BorderFactory.createEmptyBorder(10, 20, 10, 20));
        btn.setHorizontalAlignment(SwingConstants.LEFT);
        btn.addActionListener(e -> cardLayout.show(cardPanel, text));
        
        btn.addMouseListener(new MouseAdapter() {
            public void mouseEntered(MouseEvent e) { 
                btn.setBackground(new Color(210, 230, 250));
                btn.setForeground(ACCENT_BLUE);
            }
            public void mouseExited(MouseEvent e) { 
                btn.setBackground(SIDEBAR_LIGHT); 
                btn.setForeground(TEXT_DARK);
            }
        });
        return btn;
    }

    private JPanel createDashboard() {
        JPanel p = new JPanel(new BorderLayout()) {
            @Override
            protected void paintComponent(Graphics g) {
                Graphics2D g2d = (Graphics2D) g;
                g2d.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
                GradientPaint gp = new GradientPaint(0, 0, new Color(230, 240, 250), 0, getHeight(), Color.WHITE);
                g2d.setPaint(gp);
                g2d.fillRect(0, 0, getWidth(), getHeight());
            }
        };
        
        JPanel dashboardTop = new JPanel(new BorderLayout());
        dashboardTop.setOpaque(false);
        dashboardTop.setBorder(BorderFactory.createEmptyBorder(20, 40, 0, 40));
        
        JLabel status = new JLabel("● SYSTEM ACTIVE - PULKIT MODE");
        status.setForeground(SUCCESS_GREEN);
        status.setFont(boldUI);
        dashboardTop.add(status, BorderLayout.WEST);

        JLabel clock = new JLabel();
        clock.setFont(new Font("Segoe UI", Font.BOLD, 18));
        clock.setForeground(ACCENT_BLUE);
        Timer t = new Timer(1000, e -> clock.setText("🕒 " + new SimpleDateFormat("hh:mm aa").format(new Date())));
        t.start();
        dashboardTop.add(clock, BorderLayout.EAST);

        JPanel mid = new JPanel();
        mid.setLayout(new BoxLayout(mid, BoxLayout.Y_AXIS));
        mid.setOpaque(false);

        int hour = Calendar.getInstance().get(Calendar.HOUR_OF_DAY);
        welcomeText = (hour < 12) ? "Good Morning, Pulkit! ☀️" : (hour < 17) ? "Good Afternoon, Pulkit! 🌤️" : "Good Evening, Pulkit! 🌆";
        
        welcomeLabel = new JLabel(" ");
        welcomeLabel.setFont(typingFont);
        welcomeLabel.setForeground(TEXT_DARK);
        welcomeLabel.setAlignmentX(Component.CENTER_ALIGNMENT);
        welcomeLabel.setBorder(BorderFactory.createEmptyBorder(60, 0, 20, 0));

        JLabel mLabel = new JLabel("Chandigarh Success Meter 🏰");
        mLabel.setFont(boldUI);
        mLabel.setForeground(Color.GRAY);
        mLabel.setAlignmentX(Component.CENTER_ALIGNMENT);
        
        chandigarhMeter = new JProgressBar(0, 100);
        chandigarhMeter.setValue(55);
        chandigarhMeter.setStringPainted(true);
        chandigarhMeter.setForeground(ACCENT_BLUE);
        chandigarhMeter.setBackground(new Color(220, 230, 240));
        chandigarhMeter.setMaximumSize(new Dimension(500, 30));

        quoteArea = new JTextArea("");
        quoteArea.setFont(quoteFont);
        quoteArea.setForeground(new Color(100, 100, 100));
        quoteArea.setOpaque(false);
        quoteArea.setEditable(false);
        quoteArea.setLineWrap(true);
        quoteArea.setWrapStyleWord(true);
        quoteArea.setAlignmentX(Component.CENTER_ALIGNMENT);
        quoteArea.setBorder(BorderFactory.createEmptyBorder(40, 80, 20, 80));

        mid.add(welcomeLabel);
        mid.add(Box.createVerticalStrut(20));
        mid.add(mLabel);
        mid.add(Box.createVerticalStrut(10));
        mid.add(chandigarhMeter);
        mid.add(quoteArea);

        // --- 💎 ULTIMATE VIP PREMIUM 4-BUTTON SYSTEM (NO ERROR VERSION) ---
        JPanel bottom = new JPanel(new GridLayout(1, 4, 20, 0));
        bottom.setOpaque(false);
        bottom.setBorder(BorderFactory.createEmptyBorder(20, 50, 80, 50));

        // 1. DIDI'S BLESSINGS
        JButton dBtn = new JButton("💖 Didi");
        dBtn.setFont(emojiFont);
        dBtn.setBackground(new Color(255, 240, 245));
        dBtn.addActionListener(e -> {
            JDialog dDialog = new JDialog(this, true);
            dDialog.setUndecorated(true); dDialog.setSize(450, 250); dDialog.setLocationRelativeTo(this);
            dDialog.setShape(new RoundRectangle2D.Double(0, 0, 450, 250, 40, 40));

            JPanel pnlD = new JPanel(new BorderLayout()) {
                protected void paintComponent(Graphics g) {
                    Graphics2D g2 = (Graphics2D) g; g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
                    GradientPaint gp = new GradientPaint(0, 0, new Color(255, 240, 245), 0, getHeight(), Color.WHITE);
                    g2.setPaint(gp); g2.fillRoundRect(0, 0, getWidth(), getHeight(), 40, 40);
                    g2.setColor(new Color(255, 182, 193)); g2.drawRoundRect(1, 1, getWidth()-2, getHeight()-2, 40, 40);
                }
            };
            pnlD.setLayout(new BoxLayout(pnlD, BoxLayout.Y_AXIS)); pnlD.setBorder(BorderFactory.createEmptyBorder(25, 25, 25, 25));
            
            JLabel headD = new JLabel("💌 Message from Didi"); headD.setFont(boldUI); headD.setForeground(new Color(219, 112, 147)); headD.setAlignmentX(Component.CENTER_ALIGNMENT);
            
            // YAHAN NAAM BADAL DIYA 't' se 'didiTxt'
            JTextArea didiTxt = new JTextArea(didiMessages[(int)(Math.random() * didiMessages.length)]);
            didiTxt.setFont(new Font("Segoe Print", Font.BOLD, 15)); didiTxt.setWrapStyleWord(true); didiTxt.setLineWrap(true); didiTxt.setEditable(false); didiTxt.setOpaque(false);

            JButton closeD = new JButton("Shukriya Didi! ✨");
            closeD.setBackground(new Color(255, 105, 180)); closeD.setForeground(Color.WHITE); closeD.setFont(boldUI); closeD.setAlignmentX(Component.CENTER_ALIGNMENT);
            closeD.addActionListener(ex -> dDialog.dispose());

            pnlD.add(headD); pnlD.add(Box.createVerticalGlue()); pnlD.add(didiTxt); pnlD.add(Box.createVerticalGlue()); pnlD.add(closeD);
            dDialog.add(pnlD); dDialog.setVisible(true);
        });

        // 2. DAILY TARGET
        JButton tBtn = new JButton("🎯 Target");
        tBtn.setFont(emojiFont);
        tBtn.setBackground(new Color(230, 250, 235));
        tBtn.addActionListener(e -> {
            JDialog nDialog = new JDialog(this, true);
            nDialog.setUndecorated(true); nDialog.setSize(450, 400); nDialog.setLocationRelativeTo(this);
            nDialog.setShape(new RoundRectangle2D.Double(0, 0, 450, 400, 40, 40));

            JPanel pnlT = new JPanel(new BorderLayout()) {
                protected void paintComponent(Graphics g) {
                    Graphics2D g2 = (Graphics2D) g; g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
                    GradientPaint gp = new GradientPaint(0, 0, new Color(230, 255, 240), 0, getHeight(), Color.WHITE);
                    g2.setPaint(gp); g2.fillRoundRect(0, 0, getWidth(), getHeight(), 40, 40);
                    g2.setColor(new Color(150, 230, 180)); g2.drawRoundRect(1, 1, getWidth()-2, getHeight()-2, 40, 40);
                }
            };
            pnlT.setBorder(BorderFactory.createEmptyBorder(25, 25, 25, 25));

            JTextArea targetArea = new JTextArea("● "); targetArea.setFont(new Font("Segoe UI", Font.PLAIN, 18));
            targetArea.setLineWrap(true); targetArea.setWrapStyleWord(true);

            JButton saveT = new JButton("SAVE TARGET 🔒");
            saveT.setBackground(new Color(46, 139, 87)); saveT.setForeground(Color.WHITE); saveT.setFont(boldUI);
            saveT.addActionListener(ex -> {
                try (BufferedWriter bw = new BufferedWriter(new FileWriter("Target_" + new SimpleDateFormat("dd-MM_HH-mm").format(new Date()) + ".txt"))) {
                    bw.write(targetArea.getText()); bw.flush(); bw.close();
                    chandigarhMeter.setValue(Math.min(100, chandigarhMeter.getValue() + 10));
                    nDialog.dispose(); JOptionPane.showMessageDialog(this, "Target Locked! 🎯");
                } catch (Exception err) {}
            });

            pnlT.add(new JLabel("📝 Aaj ka Target:", JLabel.CENTER), BorderLayout.NORTH);
            pnlT.add(new JScrollPane(targetArea), BorderLayout.CENTER); pnlT.add(saveT, BorderLayout.SOUTH);
            nDialog.add(pnlT); nDialog.setVisible(true);
        });

        // 3. YT MUSIC (PREMIUM LOOK)
        JButton mBtn = new JButton("🎵 Music");
        mBtn.setFont(emojiFont);
        mBtn.setFocusPainted(false);
        mBtn.setBackground(new Color(235, 245, 255)); // Soft Blue
        mBtn.setBorder(BorderFactory.createLineBorder(new Color(200, 220, 240), 1));
        
        mBtn.addActionListener(e -> {
            JDialog mDialog = new JDialog(this, true);
            mDialog.setUndecorated(true); 
            mDialog.setSize(450, 250); 
            mDialog.setLocationRelativeTo(this);
            mDialog.setShape(new RoundRectangle2D.Double(0, 0, 450, 250, 40, 40));

            JPanel pnlM = new JPanel(new BorderLayout(15, 15)) {
                protected void paintComponent(Graphics g) {
                    Graphics2D g2 = (Graphics2D) g; 
                    g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
                    GradientPaint gp = new GradientPaint(0, 0, new Color(225, 240, 255), 0, getHeight(), Color.WHITE);
                    g2.setPaint(gp); g2.fillRoundRect(0, 0, getWidth(), getHeight(), 40, 40);
                    g2.setColor(ACCENT_BLUE); g2.setStroke(new BasicStroke(2));
                    g2.drawRoundRect(1, 1, getWidth()-3, getHeight()-3, 40, 40);
                }
            };
            pnlM.setBorder(BorderFactory.createEmptyBorder(25, 35, 25, 35));

            JTextField musicIn = new JTextField(); 
            musicIn.setFont(new Font("Segoe UI", Font.PLAIN, 18));
            musicIn.setBorder(BorderFactory.createCompoundBorder(
                BorderFactory.createLineBorder(new Color(200, 200, 200), 1),
                BorderFactory.createEmptyBorder(5, 10, 5, 10)));
            
            JPanel btnPanel = new JPanel(new GridLayout(1, 2, 15, 0));
            btnPanel.setOpaque(false);

            JButton goM = new JButton("PLAY 🎧");
            goM.setBackground(ACCENT_BLUE); goM.setForeground(Color.WHITE);
            goM.setFont(boldUI); goM.setFocusPainted(false);

            JButton closeM = new JButton("CLOSE ❌");
            closeM.setBackground(new Color(240, 240, 240)); 
            closeM.setForeground(TEXT_DARK);
            closeM.setFont(boldUI); closeM.setFocusPainted(false);

            goM.addActionListener(ex -> {
                try { Desktop.getDesktop().browse(new java.net.URI("https://music.youtube.com/search?q=" + musicIn.getText().replace(" ", "+"))); mDialog.dispose(); } catch (Exception ex2) {}
            });
            closeM.addActionListener(ex -> mDialog.dispose());

            btnPanel.add(goM); btnPanel.add(closeM);
            pnlM.add(new JLabel("🎵 Kaunsa song bajana hai?", JLabel.CENTER), BorderLayout.NORTH);
            pnlM.add(musicIn, BorderLayout.CENTER); 
            pnlM.add(btnPanel, BorderLayout.SOUTH);
            mDialog.add(pnlM); mDialog.setVisible(true);
        });

        // 4. YT STUDY (PREMIUM LOOK)
        JButton yBtn = new JButton("📺 Study");
        yBtn.setFont(emojiFont);
        yBtn.setFocusPainted(false);
        yBtn.setBackground(new Color(255, 240, 240)); // Soft Red
        yBtn.setBorder(BorderFactory.createLineBorder(new Color(240, 210, 210), 1));

        yBtn.addActionListener(e -> {
            JDialog sDialog = new JDialog(this, true);
            sDialog.setUndecorated(true); 
            sDialog.setSize(450, 250); 
            sDialog.setLocationRelativeTo(this);
            sDialog.setShape(new RoundRectangle2D.Double(0, 0, 450, 250, 40, 40));

            JPanel pnlS = new JPanel(new BorderLayout(15, 15)) {
                protected void paintComponent(Graphics g) {
                    Graphics2D g2 = (Graphics2D) g; 
                    g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
                    GradientPaint gp = new GradientPaint(0, 0, new Color(255, 235, 235), 0, getHeight(), Color.WHITE);
                    g2.setPaint(gp); g2.fillRoundRect(0, 0, getWidth(), getHeight(), 40, 40);
                    g2.setColor(new Color(220, 20, 60)); g2.setStroke(new BasicStroke(2));
                    g2.drawRoundRect(1, 1, getWidth()-3, getHeight()-3, 40, 40);
                }
            };
            pnlS.setBorder(BorderFactory.createEmptyBorder(25, 35, 25, 35));

            JTextField studyIn = new JTextField(); 
            studyIn.setFont(new Font("Segoe UI", Font.PLAIN, 18));
            studyIn.setBorder(BorderFactory.createCompoundBorder(
                BorderFactory.createLineBorder(new Color(200, 200, 200), 1),
                BorderFactory.createEmptyBorder(5, 10, 5, 10)));
            
            JPanel btnPanelS = new JPanel(new GridLayout(1, 2, 15, 0));
            btnPanelS.setOpaque(false);

            JButton goS = new JButton("SEARCH 📺");
            goS.setBackground(new Color(220, 20, 60)); goS.setForeground(Color.WHITE);
            goS.setFont(boldUI); goS.setFocusPainted(false);

            JButton closeS = new JButton("CLOSE ❌");
            closeS.setBackground(new Color(240, 240, 240));
            closeS.setFont(boldUI); closeS.setFocusPainted(false);

            goS.addActionListener(ex -> {
                try { Desktop.getDesktop().browse(new java.net.URI("https://www.youtube.com/results?search_query=" + studyIn.getText().replace(" ", "+"))); sDialog.dispose(); } catch (Exception ex2) {}
            });
            closeS.addActionListener(ex -> sDialog.dispose());

            btnPanelS.add(goS); btnPanelS.add(closeS);
            pnlS.add(new JLabel("📺 Aaj kya seekhna hai?", JLabel.CENTER), BorderLayout.NORTH);
            pnlS.add(studyIn, BorderLayout.CENTER); 
            pnlS.add(btnPanelS, BorderLayout.SOUTH);
            sDialog.add(pnlS); sDialog.setVisible(true);
        });

        bottom.add(dBtn); bottom.add(tBtn); bottom.add(mBtn); bottom.add(yBtn);        p.add(dashboardTop, BorderLayout.NORTH);
        p.add(mid, BorderLayout.CENTER);
        p.add(bottom, BorderLayout.SOUTH);
        return p;
    }
    private void startTypingAnimation() {
        Timer welcomeTimer = new Timer(100, new ActionListener() {
            int charIdx = 0;
            @Override
            public void actionPerformed(ActionEvent e) {
                if (charIdx <= welcomeText.length()) {
                    welcomeLabel.setText(welcomeText.substring(0, charIdx));
                    charIdx++;
                } else {
                    ((Timer)e.getSource()).stop();
                    startQuoteAnimation();
                }
            }
        });
        welcomeTimer.start();
    }

    private void startQuoteAnimation() {
        Timer quoteTimer = new Timer(50, new ActionListener() {
            int charIdx = 0;
            @Override
            public void actionPerformed(ActionEvent e) {
                if (charIdx <= motivationText.length()) {
                    quoteArea.setText(motivationText.substring(0, charIdx));
                    charIdx++;
                } else {
                    ((Timer)e.getSource()).stop();
                }
            }
        });
        quoteTimer.start();
    }

   // ==========================================
    // PREMIUM STUDY HUB (FINAL VVIP LOOK)
    // ==========================================
    private JPanel createStudyPage() {
        CardLayout studyCL = new CardLayout();
        JPanel container = new JPanel(studyCL);

        // --- 1. MAIN HUB PANEL (Cloudy Gradient & Glass Cards) ---
        JPanel hub = new JPanel(new BorderLayout(25, 25)) {
            @Override
            protected void paintComponent(Graphics g) {
                Graphics2D g2d = (Graphics2D) g;
                g2d.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
                // Wahi Premium Soft Blue to White Gradient
                GradientPaint gp = new GradientPaint(0, 0, new Color(240, 248, 255), 0, getHeight(), Color.WHITE);
                g2d.setPaint(gp);
                g2d.fillRect(0, 0, getWidth(), getHeight());
            }
        };
        hub.setBorder(BorderFactory.createEmptyBorder(30, 40, 30, 40));

        // Top Header Section
        JPanel headerPanel = new JPanel(new BorderLayout());
        headerPanel.setOpaque(false);
        JLabel header = new JLabel("✨ Saraswati Study Hub");
        header.setFont(new Font("Segoe UI", Font.BOLD, 32));
        header.setForeground(TEXT_DARK);
        JLabel subHeader = new JLabel("Pulkit, aaj kya phodna hai? 🚀");
        subHeader.setFont(new Font("Segoe UI", Font.PLAIN, 16));
        subHeader.setForeground(Color.GRAY);
        headerPanel.add(header, BorderLayout.NORTH);
        headerPanel.add(subHeader, BorderLayout.SOUTH);
        hub.add(headerPanel, BorderLayout.NORTH);

        // Center Cards Container
        JPanel cardsContainer = new JPanel(new GridLayout(1, 3, 30, 0));
        cardsContainer.setOpaque(false);

        String[] studyTopics = {"Java / BCA", "SSC Maths", "English Pro"};
        String[] subLabels = {"Object Oriented", "Arithmetic", "Vocabulary"};
        Color[] cardAccents = {ACCENT_BLUE, SUCCESS_GREEN, new Color(255, 105, 180)};

        for (int i = 0; i < studyTopics.length; i++) {
            final int index = i;
            JPanel glassCard = new JPanel() {
                protected void paintComponent(Graphics g) {
                    Graphics2D g2 = (Graphics2D) g;
                    g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
                    g2.setColor(new Color(255, 255, 255, 180)); // Frosty Effect
                    g2.fillRoundRect(0, 0, getWidth(), getHeight(), 35, 35);
                    g2.setColor(new Color(230, 230, 230)); // Subtle Border
                    g2.drawRoundRect(0, 0, getWidth()-1, getHeight()-1, 35, 35);
                }
            };
            glassCard.setLayout(new BoxLayout(glassCard, BoxLayout.Y_AXIS));
            glassCard.setOpaque(false);
            
            JLabel nameL = new JLabel(studyTopics[i]);
            nameL.setFont(new Font("Segoe UI", Font.BOLD, 22));
            nameL.setAlignmentX(Component.CENTER_ALIGNMENT);
            
            JLabel subL = new JLabel(subLabels[i]);
            subL.setFont(new Font("Segoe UI", Font.PLAIN, 14));
            subL.setForeground(Color.GRAY);
            subL.setAlignmentX(Component.CENTER_ALIGNMENT);

            JButton actionBtn = new JButton("Start Focus 🎯");
            actionBtn.setBackground(cardAccents[i]);
            actionBtn.setForeground(Color.WHITE);
            actionBtn.setFont(boldUI);
            actionBtn.setAlignmentX(Component.CENTER_ALIGNMENT);
            actionBtn.setFocusPainted(false);
            actionBtn.addActionListener(e -> {
                JPanel room = createSubMenuRoom(studyTopics[index], container, studyCL, cardAccents[index]);
                container.add(room, "ROOM_" + index);
                studyCL.show(container, "ROOM_" + index);
            });

            glassCard.add(Box.createVerticalGlue());
            glassCard.add(nameL);
            glassCard.add(Box.createVerticalStrut(8));
            glassCard.add(subL);
            glassCard.add(Box.createVerticalStrut(25));
            glassCard.add(actionBtn);
            glassCard.add(Box.createVerticalGlue());
            cardsContainer.add(glassCard);
        }
        hub.add(cardsContainer, BorderLayout.CENTER);

        // --- BOTTOM SECTION: QUICK NOTES PAD (Jo wapas chahiye tha) ---
        JPanel bottomPanel = new JPanel(new BorderLayout(20, 0));
        bottomPanel.setOpaque(false);
        bottomPanel.setBorder(BorderFactory.createEmptyBorder(20, 0, 0, 0));

        JTextArea miniPad = new JTextArea(" Today's Lesson: Study about inheritance...");
        miniPad.setFont(new Font("Segoe UI", Font.ITALIC, 15));
        miniPad.setBackground(new Color(255, 255, 255, 200));
        miniPad.setBorder(BorderFactory.createCompoundBorder(
            BorderFactory.createLineBorder(new Color(220, 230, 240), 1, true),
            BorderFactory.createEmptyBorder(15, 15, 15, 15)
        ));

        JButton syncBtn = new JButton("Save & Update Meter 🏆");
        syncBtn.setBackground(SUCCESS_GREEN);
        syncBtn.setForeground(Color.WHITE);
        syncBtn.setFont(boldUI);
        syncBtn.addActionListener(e -> {
            saveData("Study_Log.txt", "Quick Note: " + miniPad.getText());
            chandigarhMeter.setValue(Math.min(100, chandigarhMeter.getValue() + 5));
            JOptionPane.showMessageDialog(this, "Notes Saved! Chandigarh Meter Updated. ✨");
        });

        bottomPanel.add(new JScrollPane(miniPad), BorderLayout.CENTER);
        bottomPanel.add(syncBtn, BorderLayout.EAST);
        hub.add(bottomPanel, BorderLayout.SOUTH);

        container.add(hub, "HUB");
        return container;
    }
    // --- 1. REPAIRING PAGE ---
    private JPanel createRepairPage() {
        JPanel p = new JPanel(null); p.setBackground(BG_WHITE);
        JLabel l = new JLabel("Device Model:"); l.setBounds(50, 40, 200, 30);
        JTextField d = new JTextField(); d.setBounds(50, 70, 300, 35);
        JButton b = new JButton("Get Token 🎫"); b.setBounds(50, 120, 150, 40);
        b.setBackground(ACCENT_BLUE); b.setForeground(Color.WHITE);
        b.addActionListener(e -> { 
            String tk = "REP-"+(int)(Math.random()*999); 
            saveData("Repair.txt", tk + " | " + d.getText()); 
            JOptionPane.showMessageDialog(this, "Token: " + tk); 
        });
        p.add(l); p.add(d); p.add(b); return p;
    }

    // --- 2. PERSONAL PAGE ---
    private JPanel createPersonalPage() {
        JPanel p = new JPanel(null); p.setBackground(BG_WHITE);
        JLabel l = new JLabel("Dream Goal (Chandigarh 🏰):"); l.setBounds(50, 40, 300, 30);
        JTextField g = new JTextField(); g.setBounds(50, 70, 400, 35);
        JButton b = new JButton("Lock Goal ✨"); b.setBounds(50, 120, 150, 40);
        b.setBackground(SUCCESS_GREEN); b.setForeground(Color.WHITE);
        b.addActionListener(e -> { 
            saveData("Personal.txt", g.getText()); 
            JOptionPane.showMessageDialog(this, "Goal Locked! 🚀"); 
        });
        p.add(l); p.add(g); p.add(b); return p;
    }

    // --- 3. RECORDS HISTORY PAGE ---
    private JPanel createHistoryPage() {
        JPanel p = new JPanel(new BorderLayout());
        p.setBackground(BG_WHITE);
        JTextArea area = new JTextArea(); area.setEditable(false);
        JButton refresh = new JButton("🔄 Refresh All Records");
        refresh.addActionListener(e -> {
            area.setText("");
            String[] files = {"Sales.txt", "Repair.txt", "Personal.txt", "Study_Log.txt"};
            for(String f : files) {
                try (BufferedReader br = new BufferedReader(new FileReader(f))) {
                    area.append(">>> " + f.toUpperCase() + " <<<\n");
                    String line; while((line = br.readLine()) != null) area.append(line + "\n");
                    area.append("\n");
                } catch (Exception ex) {}
            }
        });
        p.add(new JScrollPane(area), BorderLayout.CENTER); p.add(refresh, BorderLayout.SOUTH);
        return p;
    }

    // --- 4. SHOP PAGE (Just in case error aaye) ---
    private JPanel createShopPage() {
        JPanel p = new JPanel(null); p.setBackground(BG_WHITE);
        JLabel l1 = new JLabel("Customer Name:"); l1.setBounds(50, 40, 200, 30);
        JTextField c = new JTextField(); c.setBounds(50, 70, 300, 35);
        JLabel l2 = new JLabel("Amount (₹):"); l2.setBounds(50, 120, 200, 30);
        JTextField a = new JTextField(); a.setBounds(50, 150, 300, 35);
        JButton b = new JButton("Bill + GST 🧾"); b.setBounds(50, 210, 150, 40);
        b.setBackground(ACCENT_BLUE); b.setForeground(Color.WHITE);
        b.addActionListener(e -> {
            try { 
                double tot = Double.parseDouble(a.getText()) * 1.18; 
                saveData("Sales.txt", c.getText()+" | ₹"+tot); 
                JOptionPane.showMessageDialog(this, "Total: ₹"+tot); 
            } catch(Exception ex) { JOptionPane.showMessageDialog(this, "Amount sahi se bharo!"); }
        });
        p.add(l1); p.add(c); p.add(l2); p.add(a); p.add(b); return p;
    }
    private void saveData(String fn, String d) {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter(fn, true))) {
            bw.write(d + " | " + new Date()); bw.newLine();
        } catch (Exception e) {}
    }
    private JPanel createChatbotPage() {
    JPanel p = new JPanel(new BorderLayout());
    p.setBackground(BG_WHITE);

    JTextArea chatArea = new JTextArea();
    chatArea.setEditable(false);
    chatArea.setFont(new Font("Segoe UI", Font.PLAIN, 15));
    chatArea.setLineWrap(true);
    chatArea.setWrapStyleWord(true);
    chatArea.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));
    
    JScrollPane scroll = new JScrollPane(chatArea);
    
    JPanel inputPanel = new JPanel(new BorderLayout());
    JTextField inputField = new JTextField();
    inputField.setFont(new Font("Segoe UI", Font.PLAIN, 16));
    JButton sendBtn = new JButton("Send 🚀");
    
    inputPanel.add(inputField, BorderLayout.CENTER);
    inputPanel.add(sendBtn, BorderLayout.EAST);

    // Chat Logic
    sendBtn.addActionListener(e -> {
        String msg = inputField.getText().toLowerCase();
        if(!msg.isEmpty()) {
            chatArea.append("Pulkit: " + msg + "\n");
            inputField.setText("");
            
            // Simple Bot Logic
            String response = "AI: Main abhi seekh raha hoon, par Pulkit bhai aap tension mat lo! 🚀";
            if(msg.contains("hello") || msg.contains("hi")) response = "AI: Hello Pulkit! System active hai. 🔐";
            else if(msg.contains("billing")) response = "AI: Shop Billing section mein jaakar entry karein. 🧾";
            else if(msg.contains("didi")) response = "AI: Didi hamesha aapke saath hain, unki blessings le lo! 💖";
            else if(msg.contains("crash")) response = "AI: GitHub use karo, code kabhi crash nahi hoga! 😎";
            
            chatArea.append(response + "\n\n");
        }
    });

    p.add(new JLabel("  🤖 Saraswati AI Assistant (Beta)", SwingConstants.LEFT), BorderLayout.NORTH);
    p.add(scroll, BorderLayout.CENTER);
    p.add(inputPanel, BorderLayout.SOUTH);
    return p;
}

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            new SaraswatiMasterApp();
        });
    }
private JPanel createSubMenuRoom(String title, JPanel parent, CardLayout mainCL, Color theme) {
        CardLayout roomCL = new CardLayout();
        JPanel roomContainer = new JPanel(roomCL);

        // --- SUB-MENU HUB (Consistent Premium Background) ---
        JPanel menuPanel = new JPanel(new GridLayout(1, 3, 30, 0)) {
            protected void paintComponent(Graphics g) {
                Graphics2D g2d = (Graphics2D) g;
                g2d.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
                GradientPaint gp = new GradientPaint(0, 0, new Color(240, 248, 255), 0, getHeight(), Color.WHITE);
                g2d.setPaint(gp); g2d.fillRect(0, 0, getWidth(), getHeight());
            }
        };
        menuPanel.setBorder(BorderFactory.createEmptyBorder(100, 50, 100, 50));

        String[] options = {"Study 📖", "Project 💻", "Test 📝"};
        String[] subDescs = {"Daily Notes", "BCA Assignments", "Mock Tests"};

        for (int i = 0; i < options.length; i++) {
            final String opt = options[i];
            final int idx = i;

            // --- COLORFUL GLASS CARD (Match with Main Theme) ---
            JPanel glassCard = new JPanel() {
                protected void paintComponent(Graphics g) {
                    Graphics2D g2 = (Graphics2D) g;
                    g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
                    
                    // Frosted White Center
                    g2.setColor(new Color(255, 255, 255, 210)); 
                    g2.fillRoundRect(0, 0, getWidth(), getHeight(), 35, 35);
                    
                    // COLORFUL GLOW BORDER (Wahi colorful effect jo tumne manga)
                    g2.setColor(theme); 
                    g2.setStroke(new BasicStroke(3)); // Thoda mota border glow ke liye
                    g2.drawRoundRect(2, 2, getWidth() - 5, getHeight() - 5, 35, 35);
                }
            };
            glassCard.setLayout(new BoxLayout(glassCard, BoxLayout.Y_AXIS));
            glassCard.setOpaque(false);

            JLabel nameL = new JLabel(opt);
            nameL.setFont(new Font("Segoe UI", Font.BOLD, 24));
            nameL.setForeground(TEXT_DARK);
            nameL.setAlignmentX(Component.CENTER_ALIGNMENT);

            JLabel descL = new JLabel(subDescs[i]);
            descL.setFont(new Font("Segoe UI", Font.PLAIN, 14));
            descL.setForeground(Color.GRAY);
            descL.setAlignmentX(Component.CENTER_ALIGNMENT);

            // Button matching the Subject's Theme
            JButton btn = new JButton("Start Now 🚀");
            btn.setBackground(theme); 
            btn.setForeground(Color.WHITE);
            btn.setFont(boldUI);
            btn.setFocusPainted(false);
            btn.setAlignmentX(Component.CENTER_ALIGNMENT);
            btn.setCursor(new Cursor(Cursor.HAND_CURSOR));
            
            btn.addActionListener(e -> {
                JPanel editor = createFinalEditor(title + " - " + opt, roomContainer, roomCL, theme);
                roomContainer.add(editor, opt);
                roomCL.show(roomContainer, opt);
            });

            glassCard.add(Box.createVerticalGlue());
            glassCard.add(nameL);
            glassCard.add(Box.createVerticalStrut(10));
            glassCard.add(descL);
            glassCard.add(Box.createVerticalStrut(25));
            glassCard.add(btn);
            glassCard.add(Box.createVerticalGlue());

            menuPanel.add(glassCard);
        }

        // --- BACK BUTTON WITH STYLE ---
        JPanel topPanel = new JPanel(new FlowLayout(FlowLayout.LEFT));
        topPanel.setOpaque(false);
        JButton backBtn = new JButton("⬅ Back to Subjects Hub");
        backBtn.setFont(boldUI);
        backBtn.setFocusPainted(false);
        backBtn.addActionListener(e -> mainCL.show(parent, "HUB"));
        topPanel.add(backBtn);

        JPanel mainLayout = new JPanel(new BorderLayout());
        mainLayout.setOpaque(false);
        mainLayout.add(topPanel, BorderLayout.NORTH);
        mainLayout.add(menuPanel, BorderLayout.CENTER);

        roomContainer.add(mainLayout, "MENU");
        return roomContainer;
    }

    // --- FINAL EDITOR FOR WRITING NOTES (Premium Consistent Look) ---
    private JPanel createFinalEditor(String head, JPanel parent, CardLayout subCL, Color theme) {
        JPanel p = new JPanel(new BorderLayout(20, 20));
        p.setBackground(Color.WHITE);
        p.setBorder(BorderFactory.createEmptyBorder(30, 30, 30, 30));

        // Header Title
        JLabel titleLbl = new JLabel(head);
        titleLbl.setFont(new Font("Segoe UI", Font.BOLD, 28));
        titleLbl.setForeground(TEXT_DARK);

        // Writing Area with Premium Border
        JTextArea area = new JTextArea("Likho Pulkit bhai...");
        area.setFont(new Font("Segoe UI", Font.PLAIN, 18));
        area.setLineWrap(true);
        area.setWrapStyleWord(true);
        area.setBorder(BorderFactory.createCompoundBorder(
            BorderFactory.createLineBorder(new Color(230, 235, 245), 1, true),
            BorderFactory.createEmptyBorder(15, 15, 15, 15)
        ));

        // Buttons Panel
        JPanel bot = new JPanel(new FlowLayout(FlowLayout.RIGHT, 15, 0));
        bot.setOpaque(false);

        JButton saveBtn = new JButton("Save & Sync 🏆");
        saveBtn.setBackground(SUCCESS_GREEN); 
        saveBtn.setForeground(Color.WHITE);
        saveBtn.setFont(boldUI);
        saveBtn.addActionListener(e -> {
            saveData("Study_Log.txt", head + ": " + area.getText());
            // Progress Bar update
            chandigarhMeter.setValue(Math.min(100, chandigarhMeter.getValue() + 5));
            JOptionPane.showMessageDialog(p, "Data Save ho gaya! Chandigarh Meter 🏰 upar gaya.");
        });

        JButton backBtn = new JButton("⬅ Back");
        backBtn.addActionListener(e -> subCL.show(parent, "MENU"));

        bot.add(backBtn); 
        bot.add(saveBtn);

        p.add(titleLbl, BorderLayout.NORTH);
        p.add(new JScrollPane(area), BorderLayout.CENTER);
        p.add(bot, BorderLayout.SOUTH);
        
        return p;
    }
}
