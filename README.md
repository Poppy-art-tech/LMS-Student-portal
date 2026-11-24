import javax.swing.*;
import javax.swing.border.EmptyBorder;
import javax.swing.table.DefaultTableModel;
import javax.swing.table.TableRowSorter;
import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.io.File;

public class LMSStudentSystem extends JFrame {
    private final Color sidebarColor = new Color(39, 31, 57);
    private final Color primary = new Color(126, 87, 194);
    private final Color primaryDark = new Color(91, 63, 145);
    private final Color background = new Color(247, 245, 250);
    private final Color surface = Color.WHITE;
    private final Color textColor = new Color(48, 42, 59);
    private final Color muted = new Color(132, 124, 145);
    private final Color borderColor = new Color(235, 230, 241);
    private final Color teal = new Color(43, 159, 145);
    private final Color orange = new Color(222, 151, 70);
    private final String studentName = "Pamela Phiri";

    private final CardLayout cardLayout = new CardLayout();
    private final JPanel contentPanel = new JPanel(cardLayout);
    private final JPanel sidebar = new JPanel();
    private final JLabel pageTitle = new JLabel("Dashboard");
    private final JLabel statusLabel = new JLabel(" ");
    private final DefaultTableModel assignmentModel = new DefaultTableModel(
            new Object[]{"Assignment", "Subject", "Due Date", "Status", "Submission File"}, 0) {
        @Override
        public boolean isCellEditable(int row, int column) {
            return false;
        }
    };

    private final String[][] courseData = {
            {"IT101", "Operating Systems", "Mr. Sharma", "4"},
            {"IT102", "Data Structures", "Ms. Gupta", "4"},
            {"IT103", "Database Management", "Mr. Kumar", "4"},
            {"IT104", "Software Engineering", "Ms. Singh", "3"},
            {"IT105", "Cyber Security", "Mr. Verma", "3"},
            {"IT106", "Blockchain Technology", "Ms. Rao", "3"}
    };

    private final String[][] attendanceData = {
            {"Operating Systems", "28", "30"},
            {"Data Structures", "25", "30"},
            {"DBMS", "27", "30"},
            {"Software Engineering", "29", "30"},
            {"Cyber Security", "26", "30"},
            {"Blockchain", "28", "30"}
    };

    private final String[][] resultData = {
            {"Operating Systems", "85", "A", "Pass"},
            {"Data Structures", "78", "B+", "Pass"},
            {"DBMS", "90", "A+", "Pass"},
            {"Software Engineering", "88", "A", "Pass"},
            {"Cyber Security", "82", "A", "Pass"},
            {"Blockchain", "91", "A+", "Pass"}
    };

    private final JButton[] navigationButtons = new JButton[6];
    private final String[] pageNames = {"Dashboard", "My Courses", "Assignments", "Attendance", "Results", "My Profile"};

    public LMSStudentSystem() {
        setTitle("Student Portal | Learning Management System");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setMinimumSize(new Dimension(1050, 680));
        setSize(1250, 790);
        setLocationRelativeTo(null);
        setLayout(new BorderLayout());
        getContentPane().setBackground(background);

        buildSidebar();
        buildContent();

        add(sidebar, BorderLayout.WEST);
        add(contentPanel, BorderLayout.CENTER);
        showPage("Dashboard", 0);
        setVisible(true);
    }

    private void buildSidebar() {
        sidebar.setBackground(sidebarColor);
        sidebar.setPreferredSize(new Dimension(235, 0));
        sidebar.setLayout(new BoxLayout(sidebar, BoxLayout.Y_AXIS));
        sidebar.setBorder(new EmptyBorder(25, 17, 22, 17));

        JLabel brand = new JLabel("  CAMPUSLY");
        brand.setFont(new Font("SansSerif", Font.BOLD, 21));
        brand.setForeground(Color.WHITE);
        brand.setAlignmentX(Component.LEFT_ALIGNMENT);

        JLabel subtitle = new JLabel("STUDENT PORTAL");
        subtitle.setFont(new Font("SansSerif", Font.BOLD, 10));
        subtitle.setForeground(new Color(184, 170, 204));
        subtitle.setBorder(new EmptyBorder(5, 12, 28, 0));
        subtitle.setAlignmentX(Component.LEFT_ALIGNMENT);

        sidebar.add(brand);
        sidebar.add(subtitle);

        String[] labels = {"Dashboard", "My Courses", "Assignments", "Attendance", "Results", "My Profile"};
        String[] symbols = {"⌂", "▤", "▣", "◷", "▥", "◉"};

        for (int i = 0; i < labels.length; i++) {
            final int index = i;
            JButton button = new JButton(symbols[i] + "   " + labels[i]);
            button.setFont(new Font("SansSerif", Font.PLAIN, 14));
            button.setForeground(new Color(221, 214, 232));
            button.setBackground(sidebarColor);
            button.setHorizontalAlignment(SwingConstants.LEFT);
            button.setFocusPainted(false);
            button.setBorderPainted(false);
            button.setOpaque(true);
            button.setCursor(Cursor.getPredefinedCursor(Cursor.HAND_CURSOR));
            button.setMaximumSize(new Dimension(Integer.MAX_VALUE, 47));
            button.setPreferredSize(new Dimension(195, 47));
            button.setBorder(new EmptyBorder(0, 13, 0, 8));
            button.addActionListener(e -> showPage(pageNames[index], index));
            navigationButtons[i] = button;
            sidebar.add(button);
            sidebar.add(Box.createVerticalStrut(6));
        }

        sidebar.add(Box.createVerticalGlue());

        RoundedPanel userPanel = new RoundedPanel(new Color(57, 47, 77), 16);
        userPanel.setLayout(new BorderLayout(10, 0));
        userPanel.setBorder(new EmptyBorder(12, 11, 12, 11));
        JLabel initials = new JLabel("PP", SwingConstants.CENTER);
        initials.setOpaque(true);
        initials.setBackground(new Color(166, 133, 216));
        initials.setForeground(Color.WHITE);
        initials.setFont(new Font("SansSerif", Font.BOLD, 13));
        initials.setPreferredSize(new Dimension(37, 37));
        JLabel userText = new JLabel("<html><b style='color:#ffffff'>Pamela Phiri</b><br><span style='color:#c7bdd4;font-size:10px'>BSIT · Semester 2</span></html>");
        userPanel.add(initials, BorderLayout.WEST);
        userPanel.add(userText, BorderLayout.CENTER);
        userPanel.setMaximumSize(new Dimension(Integer.MAX_VALUE, 65));
        sidebar.add(userPanel);
        sidebar.add(Box.createVerticalStrut(12));

        JButton logout = new JButton("↪   Log out");
        logout.setAlignmentX(Component.LEFT_ALIGNMENT);
        logout.setForeground(new Color(245, 190, 198));
        logout.setBackground(sidebarColor);
        logout.setFont(new Font("SansSerif", Font.PLAIN, 13));
        logout.setBorder(new EmptyBorder(10, 13, 10, 10));
        logout.setFocusPainted(false);
        logout.setCursor(Cursor.getPredefinedCursor(Cursor.HAND_CURSOR));
        logout.addActionListener(e -> {
            int choice = JOptionPane.showConfirmDialog(this, "Are you sure you want to log out?",
                    "Confirm logout", JOptionPane.YES_NO_OPTION);
            if (choice == JOptionPane.YES_OPTION) {
                dispose();
            }
        });
        sidebar.add(logout);
    }

    private void buildContent() {
        contentPanel.setBackground(background);
        contentPanel.add(createDashboard(), "Dashboard");
        contentPanel.add(createCoursesPage(), "My Courses");
        contentPanel.add(createAssignmentsPage(), "Assignments");
        contentPanel.add(createAttendancePage(), "Attendance");
        contentPanel.add(createResultsPage(), "Results");
        contentPanel.add(createProfilePage(), "My Profile");
    }

    private JPanel createShell(String title, String description) {
        JPanel page = new JPanel(new BorderLayout(0, 22));
        page.setBackground(background);
        page.setBorder(new EmptyBorder(28, 32, 28, 32));

        JPanel header = new JPanel(new BorderLayout());
        header.setOpaque(false);

        JPanel headingBox = new JPanel();
        headingBox.setOpaque(false);
        headingBox.setLayout(new BoxLayout(headingBox, BoxLayout.Y_AXIS));

        JLabel heading = new JLabel(title);
        heading.setFont(new Font("SansSerif", Font.BOLD, 27));
        heading.setForeground(textColor);

        JLabel subheading = new JLabel(description);
        subheading.setFont(new Font("SansSerif", Font.PLAIN, 13));
        subheading.setForeground(muted);
        subheading.setBorder(new EmptyBorder(5, 1, 0, 0));

        headingBox.add(heading);
        headingBox.add(subheading);
        header.add(headingBox, BorderLayout.WEST);

        JLabel date = new JLabel("STUDENT WORKSPACE");
        date.setFont(new Font("SansSerif", Font.BOLD, 10));
        date.setForeground(primary);
        header.add(date, BorderLayout.EAST);

        page.add(header, BorderLayout.NORTH);
        return page;
    }

    private JPanel createDashboard() {
        JPanel page = createShell("Good day, Pamela", "Here is your academic overview for this semester.");

        JPanel body = new JPanel();
        body.setOpaque(false);
        body.setLayout(new BoxLayout(body, BoxLayout.Y_AXIS));

        RoundedPanel welcome = new RoundedPanel(new Color(239, 231, 250), 20);
        welcome.setLayout(new BorderLayout(12, 8));
        welcome.setBorder(new EmptyBorder(22, 24, 22, 24));
        JPanel welcomeText = new JPanel();
        welcomeText.setOpaque(false);
        welcomeText.setLayout(new BoxLayout(welcomeText, BoxLayout.Y_AXIS));
        JLabel welcomeTitle = new JLabel("Keep making progress!");
        welcomeTitle.setFont(new Font("SansSerif", Font.BOLD, 20));
        welcomeTitle.setForeground(new Color(76, 53, 112));
        JLabel welcomeDesc = new JLabel("Your learning journey is moving forward. Review your tasks and stay on track.");
        welcomeDesc.setFont(new Font("SansSerif", Font.PLAIN, 13));
        welcomeDesc.setForeground(new Color(105, 87, 128));
        welcomeDesc.setBorder(new EmptyBorder(7, 0, 0, 0));
        welcomeText.add(welcomeTitle);
        welcomeText.add(welcomeDesc);
        JButton assignmentsButton = primaryButton("View assignments");
        assignmentsButton.addActionListener(e -> showPage("Assignments", 2));
        welcome.add(welcomeText, BorderLayout.CENTER);
        welcome.add(assignmentsButton, BorderLayout.EAST);
        welcome.setMaximumSize(new Dimension(Integer.MAX_VALUE, 112));
        body.add(welcome);
        body.add(Box.createVerticalStrut(20));

        JPanel stats = new JPanel(new GridLayout(1, 4, 14, 0));
        stats.setOpaque(false);
        stats.add(createStatCard("ENROLLED COURSES", "6", "Active subjects", primary));
        stats.add(createStatCard("ASSIGNMENTS", pendingAssignments() + "", "Pending submission", orange));
        stats.add(createStatCard("ATTENDANCE", "92%", "Overall average", teal));
        stats.add(createStatCard("AVERAGE MARKS", "85.7%", "Across 6 subjects", new Color(206, 104, 145)));
        stats.setMaximumSize(new Dimension(Integer.MAX_VALUE, 125));
        body.add(stats);
        body.add(Box.createVerticalStrut(20));

        JPanel lower = new JPanel(new GridLayout(1, 2, 14, 0));
        lower.setOpaque(false);
        lower.add(createDashboardCourses());
        lower.add(createAnnouncements());
        lower.setPreferredSize(new Dimension(700, 265));
        lower.setMaximumSize(new Dimension(Integer.MAX_VALUE, 265));
        body.add(lower);

        JScrollPane scroll = new JScrollPane(body);
        scroll.setBorder(null);
        scroll.getViewport().setBackground(background);
        scroll.getVerticalScrollBar().setUnitIncrement(16);
        page.add(scroll, BorderLayout.CENTER);
        return page;
    }

    private JPanel createStatCard(String title, String value, String note, Color accent) {
        RoundedPanel card = new RoundedPanel(surface, 17);
        card.setLayout(new BoxLayout(card, BoxLayout.Y_AXIS));
        card.setBorder(new EmptyBorder(17, 17, 15, 17));
        JLabel titleLabel = new JLabel(title);
        titleLabel.setFont(new Font("SansSerif", Font.BOLD, 10));
        titleLabel.setForeground(muted);
        JLabel valueLabel = new JLabel(value);
        valueLabel.setFont(new Font("SansSerif", Font.BOLD, 29));
        valueLabel.setForeground(accent);
        valueLabel.setBorder(new EmptyBorder(9, 0, 3, 0));
        JLabel noteLabel = new JLabel(note);
        noteLabel.setFont(new Font("SansSerif", Font.PLAIN, 11));
        noteLabel.setForeground(muted);
        card.add(titleLabel);
        card.add(valueLabel);
        card.add(noteLabel);
        return card;
    }

    private JPanel createDashboardCourses() {
        RoundedPanel panel = new RoundedPanel(surface, 17);
        panel.setLayout(new BorderLayout(0, 13));
        panel.setBorder(new EmptyBorder(18, 18, 18, 18));
        JLabel title = sectionTitle("Your courses");
        panel.add(title, BorderLayout.NORTH);

        JPanel rows = new JPanel();
        rows.setOpaque(false);
        rows.setLayout(new BoxLayout(rows, BoxLayout.Y_AXIS));
        String[] names = {"Operating Systems", "Data Structures", "Database Management", "Software Engineering"};
        String[] codes = {"IT101", "IT102", "IT103", "IT104"};
        for (int i = 0; i < names.length; i++) {
            JPanel row = new JPanel(new BorderLayout());
            row.setOpaque(false);
            row.setBorder(new EmptyBorder(7, 0, 7, 0));
            JLabel name = new JLabel(names[i]);
            name.setFont(new Font("SansSerif", Font.BOLD, 12));
            name.setForeground(textColor);
            JLabel code = new JLabel(codes[i]);
            code.setFont(new Font("SansSerif", Font.PLAIN, 11));
            code.setForeground(muted);
            row.add(name, BorderLayout.CENTER);
            row.add(code, BorderLayout.EAST);
            rows.add(row);
            if (i < names.length - 1) {
                JSeparator separator = new JSeparator();
                separator.setForeground(borderColor);
                rows.add(separator);
            }
        }
        panel.add(rows, BorderLayout.CENTER);
        return panel;
    }

    private JPanel createAnnouncements() {
        RoundedPanel panel = new RoundedPanel(surface, 17);
        panel.setLayout(new BorderLayout(0, 13));
        panel.setBorder(new EmptyBorder(18, 18, 18, 18));
        panel.add(sectionTitle("Latest announcements"), BorderLayout.NORTH);

        JPanel notices = new JPanel();
        notices.setOpaque(false);
        notices.setLayout(new BoxLayout(notices, BoxLayout.Y_AXIS));
        notices.add(createNotice("Examination timetable", "The semester examination timetable will be announced soon.", primary));
        notices.add(Box.createVerticalStrut(13));
        notices.add(createNotice("Assignment reminders", "Complete pending assignments before their deadlines.", orange));
        notices.add(Box.createVerticalStrut(13));
        notices.add(createNotice("Attendance update", "Check your attendance regularly for every subject.", teal));
        panel.add(notices, BorderLayout.CENTER);
        return panel;
    }

    private JPanel createNotice(String title, String message, Color accent) {
        JPanel item = new JPanel(new BorderLayout(10, 0));
        item.setOpaque(false);
        JPanel marker = new JPanel();
        marker.setBackground(accent);
        marker.setPreferredSize(new Dimension(4, 42));
        JLabel heading = new JLabel(title);
        heading.setFont(new Font("SansSerif", Font.BOLD, 12));
        heading.setForeground(textColor);
        JLabel description = new JLabel("<html><div style='width:230px'>" + message + "</div></html>");
        description.setFont(new Font("SansSerif", Font.PLAIN, 11));
        description.setForeground(muted);
        JPanel labels = new JPanel();
        labels.setOpaque(false);
        labels.setLayout(new BoxLayout(labels, BoxLayout.Y_AXIS));
        labels.add(heading);
        labels.add(Box.createVerticalStrut(4));
        labels.add(description);
        item.add(marker, BorderLayout.WEST);
        item.add(labels, BorderLayout.CENTER);
        return item;
    }

    private JPanel createCoursesPage() {
        JPanel page = createShell("My Courses", "Explore the subjects you are enrolled in.");
        String[] columns = {"Course ID", "Course Name", "Instructor", "Credits"};
        JTable table = createTable(columns, courseData);
        page.add(tablePanel(table, "Search courses..."), BorderLayout.CENTER);
        return page;
    }

    private JPanel createAssignmentsPage() {
        JPanel page = createShell("Assignments", "Track deadlines and submit your completed work.");
        assignmentModel.setRowCount(0);
        assignmentModel.addRow(new Object[]{"Java Practical", "Java Programming", "25 Nov 2026", "Pending", "Not submitted"});
        assignmentModel.addRow(new Object[]{"OS Assignment", "Operating Systems", "27 Nov 2026", "Submitted", "os_assignment.pdf"});
        assignmentModel.addRow(new Object[]{"DBMS Project", "Database Management", "29 Nov 2026", "Pending", "Not submitted"});
        assignmentModel.addRow(new Object[]{"Cyber Security", "Cyber Security", "30 Nov 2026", "Submitted", "cybersecurity.docx"});

        JPanel wrapper = new JPanel(new BorderLayout(0, 14));
        wrapper.setOpaque(false);
        JTable table = new JTable(assignmentModel);
        styleTable(table);
        table.setSelectionMode(ListSelectionModel.SINGLE_SELECTION);
        wrapper.add(tablePanel(table, "Search assignments..."), BorderLayout.CENTER);

        JPanel actions = new JPanel(new FlowLayout(FlowLayout.LEFT, 10, 0));
        actions.setOpaque(false);
        JButton submit = primaryButton("Submit selected assignment");
        JButton details = secondaryButton("View submission details");
        submit.addActionListener(e -> submitAssignment(table));
        details.addActionListener(e -> showAssignmentDetails(table));
        actions.add(submit);
        actions.add(details);
        wrapper.add(actions, BorderLayout.SOUTH);

        page.add(wrapper, BorderLayout.CENTER);
        return page;
    }

    private JPanel createAttendancePage() {
        JPanel page = createShell("Attendance", "Monitor your class participation across all subjects.");
        Object[][] data = new Object[attendanceData.length][4];
        for (int i = 0; i < attendanceData.length; i++) {
            int attended = Integer.parseInt(attendanceData[i][1]);
            int total = Integer.parseInt(attendanceData[i][2]);
            data[i][0] = attendanceData[i][0];
            data[i][1] = attended;
            data[i][2] = total;
            data[i][3] = Math.round(attended * 100.0 / total) + "%";
        }
        JTable table = createTable(new String[]{"Subject", "Classes Attended", "Total Classes", "Attendance"}, data);
        page.add(tablePanel(table, "Search subjects..."), BorderLayout.CENTER);
        return page;
    }

    private JPanel createResultsPage() {
        JPanel page = createShell("Academic Results", "Review your marks and semester performance.");
        JTable table = createTable(new String[]{"Subject", "Marks", "Grade", "Result"}, resultData);
        page.add(tablePanel(table, "Search results..."), BorderLayout.CENTER);
        return page;
    }

    private JPanel createProfilePage() {
        JPanel page = createShell("My Profile", "Your student information and account details.");
        RoundedPanel profile = new RoundedPanel(surface, 18);
        profile.setLayout(new BorderLayout(18, 18));
        profile.setBorder(new EmptyBorder(25, 25, 25, 25));

        JPanel identity = new JPanel(new BorderLayout(15, 0));
        identity.setOpaque(false);
        JLabel avatar = new JLabel("PP", SwingConstants.CENTER);
        avatar.setOpaque(true);
        avatar.setBackground(new Color(239, 231, 250));
        avatar.setForeground(primaryDark);
        avatar.setFont(new Font("SansSerif", Font.BOLD, 25));
        avatar.setPreferredSize(new Dimension(76, 76));
        JPanel identityText = new JPanel();
        identityText.setOpaque(false);
        identityText.setLayout(new BoxLayout(identityText, BoxLayout.Y_AXIS));
        JLabel name = new JLabel(studentName);
        name.setFont(new Font("SansSerif", Font.BOLD, 21));
        name.setForeground(textColor);
        JLabel course = new JLabel("B.Sc. Information Technology · Semester 2");
        course.setFont(new Font("SansSerif", Font.PLAIN, 13));
        course.setForeground(muted);
        identityText.add(name);
        identityText.add(Box.createVerticalStrut(7));
        identityText.add(course);
        identity.add(avatar, BorderLayout.WEST);
        identity.add(identityText, BorderLayout.CENTER);
        profile.add(identity, BorderLayout.NORTH);

        JPanel fields = new JPanel(new GridLayout(0, 2, 18, 17));
        fields.setOpaque(false);
        addProfileField(fields, "STUDENT NAME", studentName);
        addProfileField(fields, "ROLL NUMBER", "2026IT101");
        addProfileField(fields, "PROGRAMME", "B.Sc. Information Technology");
        addProfileField(fields, "SEMESTER", "Second Semester");
        addProfileField(fields, "INSTITUTION", "Manav Rachna");
        addProfileField(fields, "EMAIL", "student@example.com");
        addProfileField(fields, "NATIONALITY", "Zimbabwean");
        profile.add(fields, BorderLayout.CENTER);

        JButton edit = secondaryButton("Edit contact details");
        edit.addActionListener(e -> editContactDetails());
        JPanel bottom = new JPanel(new FlowLayout(FlowLayout.LEFT, 0, 0));
        bottom.setOpaque(false);
        bottom.add(edit);
        profile.add(bottom, BorderLayout.SOUTH);

        page.add(profile, BorderLayout.CENTER);
        return page;
    }

    private void addProfileField(JPanel panel, String label, String value) {
        JPanel field = new JPanel();
        field.setOpaque(false);
        field.setLayout(new BoxLayout(field, BoxLayout.Y_AXIS));
        JLabel title = new JLabel(label);
        title.setFont(new Font("SansSerif", Font.BOLD, 10));
        title.setForeground(muted);
        JLabel content = new JLabel(value);
        content.setFont(new Font("SansSerif", Font.PLAIN, 14));
        content.setForeground(textColor);
        content.setBorder(new EmptyBorder(6, 0, 0, 0));
        field.add(title);
        field.add(content);
        panel.add(field);
    }

    private void editContactDetails() {
        JTextField email = new JTextField("student@example.com");
        JTextField phone = new JTextField();
        Object[] fields = {"Email address:", email, "Phone number:", phone};
        int result = JOptionPane.showConfirmDialog(this, fields, "Update contact details",
                JOptionPane.OK_CANCEL_OPTION, JOptionPane.PLAIN_MESSAGE);
        if (result == JOptionPane.OK_OPTION) {
            JOptionPane.showMessageDialog(this, "Contact details updated for this session.",
                    "Profile updated", JOptionPane.INFORMATION_MESSAGE);
        }
    }

    private JPanel tablePanel(JTable table, String placeholder) {
        RoundedPanel container = new RoundedPanel(surface, 17);
        container.setLayout(new BorderLayout(0, 14));
        container.setBorder(new EmptyBorder(18, 18, 18, 18));

        JPanel toolbar = new JPanel(new BorderLayout(12, 0));
        toolbar.setOpaque(false);
        JLabel label = sectionTitle("Records");
        JTextField search = new JTextField();
        search.setFont(new Font("SansSerif", Font.PLAIN, 13));
        search.setForeground(textColor);
        search.setBorder(BorderFactory.createCompoundBorder(
                BorderFactory.createLineBorder(borderColor, 1, true),
                new EmptyBorder(9, 12, 9, 12)));
        search.setPreferredSize(new Dimension(230, 38));
        search.putClientProperty("JTextField.placeholderText", placeholder);
        toolbar.add(label, BorderLayout.WEST);
        toolbar.add(search, BorderLayout.EAST);

        if (table.getRowSorter() == null) {
            TableRowSorter sorter = new TableRowSorter(table.getModel());
            table.setRowSorter(sorter);
            search.getDocument().addDocumentListener(new javax.swing.event.DocumentListener() {
                private void filter() {
                    String value = search.getText().trim();
                    sorter.setRowFilter(value.isEmpty() ? null : RowFilter.regexFilter("(?i)" + java.util.regex.Pattern.quote(value)));
                }
                public void insertUpdate(javax.swing.event.DocumentEvent e) { filter(); }
                public void removeUpdate(javax.swing.event.DocumentEvent e) { filter(); }
                public void changedUpdate(javax.swing.event.DocumentEvent e) { filter(); }
            });
        }

        JScrollPane scroll = new JScrollPane(table);
        scroll.setBorder(BorderFactory.createLineBorder(borderColor, 1));
        scroll.getViewport().setBackground(Color.WHITE);
        scroll.getVerticalScrollBar().setUnitIncrement(14);
        container.add(toolbar, BorderLayout.NORTH);
        container.add(scroll, BorderLayout.CENTER);
        return container;
    }

    private JTable createTable(String[] columns, Object[][] data) {
        DefaultTableModel model = new DefaultTableModel(data, columns) {
            @Override
            public boolean isCellEditable(int row, int column) {
                return false;
            }
        };
        JTable table = new JTable(model);
        styleTable(table);
        return table;
    }

    private void styleTable(JTable table) {
        table.setFont(new Font("SansSerif", Font.PLAIN, 13));
        table.setForeground(textColor);
        table.setBackground(Color.WHITE);
        table.setRowHeight(43);
        table.setShowVerticalLines(false);
        table.setShowHorizontalLines(true);
        table.setGridColor(borderColor);
        table.setIntercellSpacing(new Dimension(0, 1));
        table.setSelectionBackground(new Color(239, 231, 250));
        table.setSelectionForeground(textColor);
        table.setFillsViewportHeight(true);
        table.getTableHeader().setFont(new Font("SansSerif", Font.BOLD, 11));
        table.getTableHeader().setForeground(muted);
        table.getTableHeader().setBackground(new Color(249, 247, 251));
        table.getTableHeader().setPreferredSize(new Dimension(0, 42));
        table.getTableHeader().setBorder(BorderFactory.createMatteBorder(0, 0, 1, 0, borderColor));
        table.setAutoCreateRowSorter(true);
    }

    private void submitAssignment(JTable table) {
        int selected = table.getSelectedRow();
        if (selected < 0) {
            JOptionPane.showMessageDialog(this, "Select an assignment from the table first.",
                    "No assignment selected", JOptionPane.WARNING_MESSAGE);
            return;
        }

        int modelRow = table.convertRowIndexToModel(selected);
        String status = String.valueOf(assignmentModel.getValueAt(modelRow, 3));
        if ("Submitted".equalsIgnoreCase(status)) {
            JOptionPane.showMessageDialog(this, "This assignment has already been submitted.",
                    "Already submitted", JOptionPane.INFORMATION_MESSAGE);
            return;
        }

        JFileChooser chooser = new JFileChooser();
        chooser.setDialogTitle("Choose your assignment file");
        chooser.setFileSelectionMode(JFileChooser.FILES_ONLY);
        int result = chooser.showOpenDialog(this);
        if (result == JFileChooser.APPROVE_OPTION) {
            File file = chooser.getSelectedFile();
            assignmentModel.setValueAt("Submitted", modelRow, 3);
            assignmentModel.setValueAt(file.getName(), modelRow, 4);
            JOptionPane.showMessageDialog(this,
                    "File attached successfully for this session:\n" + file.getName(),
                    "Submission recorded", JOptionPane.INFORMATION_MESSAGE);
        }
    }

    private void showAssignmentDetails(JTable table) {
        int selected = table.getSelectedRow();
        if (selected < 0) {
            JOptionPane.showMessageDialog(this, "Select an assignment to view its details.",
                    "No assignment selected", JOptionPane.WARNING_MESSAGE);
            return;
        }
        int row = table.convertRowIndexToModel(selected);
        JOptionPane.showMessageDialog(this,
                "Assignment: " + assignmentModel.getValueAt(row, 0) +
                "\nSubject: " + assignmentModel.getValueAt(row, 1) +
                "\nDue date: " + assignmentModel.getValueAt(row, 2) +
                "\nStatus: " + assignmentModel.getValueAt(row, 3) +
                "\nFile: " + assignmentModel.getValueAt(row, 4),
                "Assignment details", JOptionPane.INFORMATION_MESSAGE);
    }

    private int pendingAssignments() {
        int pending = 0;
        for (int i = 0; i < assignmentModel.getRowCount(); i++) {
            if ("Pending".equalsIgnoreCase(String.valueOf(assignmentModel.getValueAt(i, 3)))) {
                pending++;
            }
        }
        return pending;
    }

    private JLabel sectionTitle(String text) {
        JLabel label = new JLabel(text);
        label.setFont(new Font("SansSerif", Font.BOLD, 15));
        label.setForeground(textColor);
        return label;
    }

    private JButton primaryButton(String text) {
        JButton button = new JButton(text);
        button.setFont(new Font("SansSerif", Font.BOLD, 12));
        button.setForeground(Color.WHITE);
        button.setBackground(primary);
        button.setFocusPainted(false);
        button.setBorderPainted(false);
        button.setCursor(Cursor.getPredefinedCursor(Cursor.HAND_CURSOR));
        button.setBorder(new EmptyBorder(11, 16, 11, 16));
        button.addMouseListener(new MouseAdapter() {
            public void mouseEntered(MouseEvent e) { button.setBackground(primaryDark); }
            public void mouseExited(MouseEvent e) { button.setBackground(primary); }
        });
        return button;
    }

    private JButton secondaryButton(String text) {
        JButton button = new JButton(text);
        button.setFont(new Font("SansSerif", Font.BOLD, 12));
        button.setForeground(primaryDark);
        button.setBackground(new Color(244, 239, 250));
        button.setFocusPainted(false);
        button.setBorder(BorderFactory.createLineBorder(new Color(225, 215, 239)));
        button.setCursor(Cursor.getPredefinedCursor(Cursor.HAND_CURSOR));
        button.setBorder(BorderFactory.createCompoundBorder(
                BorderFactory.createLineBorder(new Color(225, 215, 239)),
                new EmptyBorder(10, 14, 10, 14)));
        return button;
    }

    private void showPage(String page, int activeIndex) {
        pageTitle.setText(page);
        cardLayout.show(contentPanel, page);
        for (int i = 0; i < navigationButtons.length; i++) {
            JButton button = navigationButtons[i];
            if (button != null) {
                button.setBackground(i == activeIndex ? primary : sidebarColor);
                button.setForeground(i == activeIndex ? Color.WHITE : new Color(221, 214, 232));
            }
        }
    }

    private static class RoundedPanel extends JPanel {
        private final Color fill;
        private final int radius;

        RoundedPanel(Color fill, int radius) {
            this.fill = fill;
            this.radius = radius;
            setOpaque(false);
        }

        @Override
        protected void paintComponent(Graphics graphics) {
            Graphics2D g2 = (Graphics2D) graphics.create();
            g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
            g2.setColor(fill);
            g2.fillRoundRect(0, 0, getWidth(), getHeight(), radius, radius);
            g2.dispose();
            super.paintComponent(graphics);
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(LMSStudentSystem::new);
    }
}
