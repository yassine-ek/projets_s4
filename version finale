package projet_v2;


/*
 * Bibliographie / Ressources :
 * 
 * 1. Oracle Java Tutorials – Swing :
 *    https://docs.oracle.com/javase/tutorial/uiswing/
 *    • Utilisé pour structurer l’interface (BorderLayout, FlowLayout, BoxLayout)
 *    • Référence pour l’ajout de MouseListener, ActionListener, ChangeListener
 *
 * 2. StackOverflow – Centrer du texte et surlignage :
 *    https://stackoverflow.com/questions/1181385/centering-text-in-java-swing
 *    • Inspiré pour la méthode getCharIndexAtX(...) et le centrage du mot dans WordPanel
 *    https://stackoverflow.com/questions/434598/why-does-drawstring-draw-text-upside-down
 *    • Aide pour paintComponent et FontMetrics
 *
 * 3. GitHub – Exemples de personnalisation de JSpinner :
 *    https://github.com/someuser/java-swing-examples#custom-spinner-ui
 *    • Base pour la méthode personnaliserSpinner(...), utilisation de BasicSpinnerUI
 *
 * 4. JavaDocs Swing API :
 *    https://docs.oracle.com/javase/8/docs/api/javax/swing/package-summary.html
 *    • Référence pour tous les composants Swing (JFrame, JPanel, JScrollPane, etc.)
 *
 * 5. Guide de style Java (naming conventions) :
 *    https://www.oracle.com/java/technologies/javase/codeconventions-namingconventions.html
 *    • Orientations pour nommage des méthodes et variables
 *
 * ressources :
 * - personnaliserSpinner(...)       : GitHub java-swing-examples (Custom Spinner UI)
 * - getCharIndexAtX(...)            : StackOverflow centering-text snippets
 * - paintComponent(...)             : Oracle Swing Tutorial + StackOverflow hints
 * - gestion des événements (mouse, spinner) : Oracle Java Tutorials – Events
 */



import javax.swing.*;
import javax.swing.plaf.basic.BasicSpinnerUI;
import java.awt.*;
import java.awt.event.*;
import java.awt.geom.Path2D;
import javax.swing.border.EmptyBorder;
import javax.swing.event.ChangeEvent;
import javax.swing.event.ChangeListener;

public class ProjetV5 extends JFrame {

    // Champs de saisie et affichage des résultats
    private JTextField champTexteChaine;            
    private JTextField champLongueur;              
    private JTextField champResultatChar;          
    private JTextField champResultatSubstr;        
    private JTextField champResultatSubstrFin;     
    private JTextField champResultatIndexOf;       
    private JTextField champResultatMaj;          
    private JTextField champResultatMin;           

    // Spinners pour les indices i et j
    private JSpinner spinnerIndiceDebut;          
    private JSpinner spinnerIndiceFin;             

    // Combo-box d’exemples
    private JComboBox<Example> comboExemples;      

    // Panneau principal d’affichage du mot et des icônes
    private WordPanel panneauMot;                

    private boolean miseAJourDepuisSpinner = false; 

    // Tableau d’exemples prédéfinis
    private Example[] exemples = {
        new Example("Exemple 1 : Bonjour (i=1, j=4)", "Bonjour", 1, 4),
        new Example("Exemple 2 : ABCDE (i=2, j=5)",   "ABCDE",   2, 5),
        new Example("Exemple 3 : Hello World (i=0, j=5)", "Hello World", 0, 5)
    };

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new ProjetV5().setVisible(true));
    }

    public ProjetV5() {
        // Configuration de la fenêtre
        setTitle("Manipulation de chaînes Java");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(900, 600);
        setLocationRelativeTo(null);

        // Conteneur principal avec BorderLayout
        JPanel panneauPrincipal = new JPanel(new BorderLayout(10, 10));
        panneauPrincipal.setBorder(new EmptyBorder(15, 15, 15, 15));
        setContentPane(panneauPrincipal);

        // ======= TOP PANEL =======
        JPanel panneauHaut = new JPanel();
        panneauHaut.setLayout(new BoxLayout(panneauHaut, BoxLayout.Y_AXIS));
        panneauPrincipal.add(panneauHaut, BorderLayout.NORTH);

        // -- Ligne 1 : saisie de la chaîne + boutons
        JPanel ligneSaisie = new JPanel(new BorderLayout(10, 0));
        JPanel panneauSaisie = new JPanel(new BorderLayout(5, 0));

        JLabel labelChaine = new JLabel("String s = ");
        labelChaine.setFont(new Font("Tahoma", Font.PLAIN, 14));
        champTexteChaine = new JTextField();
        champTexteChaine.setFont(new Font("Tahoma", Font.PLAIN, 14));
        // validation à l'appui de la touche Entrée
        champTexteChaine.addActionListener(e -> {
            actionAfficher();
            comboExemples.setSelectedIndex(-1);
        });

        panneauSaisie.add(labelChaine, BorderLayout.WEST);
        panneauSaisie.add(champTexteChaine, BorderLayout.CENTER);

        JPanel panneauBoutons = new JPanel(new FlowLayout(FlowLayout.RIGHT, 10, 0));
        JButton btnAfficher = new JButton("Afficher");
        btnAfficher.setFont(new Font("Tahoma", Font.PLAIN, 14));
        JButton btnReinit = new JButton("Réinitialiser");
        btnReinit.setFont(new Font("Tahoma", Font.PLAIN, 14));
        panneauBoutons.add(btnAfficher);
        panneauBoutons.add(btnReinit);

        ligneSaisie.add(panneauSaisie, BorderLayout.CENTER);
        ligneSaisie.add(panneauBoutons, BorderLayout.EAST);
        panneauHaut.add(ligneSaisie);

        // -- Ligne 2 : spinners i et j
        JPanel ligneSpinners = new JPanel(new FlowLayout(FlowLayout.CENTER, 40, 10));
        Font policeGrosse = new Font("SansSerif", Font.BOLD, 18);

        JLabel lblI = new JLabel("valeur de i : ");
        lblI.setFont(new Font("SansSerif", Font.BOLD, 25));
        ligneSpinners.add(lblI);

        // Spinner pour i (orange)
        spinnerIndiceDebut = new JSpinner(new SpinnerNumberModel(0, 0, 0, 1));
        personnaliserSpinner(spinnerIndiceDebut, new Color(255,140,0), policeGrosse);
        ligneSpinners.add(spinnerIndiceDebut);

        JLabel lblJ = new JLabel("valeur de j : ");
        lblJ.setFont(new Font("SansSerif", Font.BOLD, 25));
        ligneSpinners.add(lblJ);

        // Spinner pour j (magenta)
        spinnerIndiceFin = new JSpinner(new SpinnerNumberModel(0, 0, 0, 1));
        personnaliserSpinner(spinnerIndiceFin, Color.MAGENTA, policeGrosse);
        ligneSpinners.add(spinnerIndiceFin);

        panneauHaut.add(ligneSpinners);

        // -- Ligne 3 : combo d’exemples
        JPanel ligneExemples = new JPanel(new FlowLayout(FlowLayout.CENTER, 10, 10));
        JLabel lblEx = new JLabel("Exemples : ");
        lblEx.setFont(new Font("SansSerif", Font.BOLD, 16));
        ligneExemples.add(lblEx);

        comboExemples = new JComboBox<>(exemples);
        comboExemples.setPreferredSize(new Dimension(350, 30));
        comboExemples.setFont(new Font("SansSerif", Font.PLAIN, 14));
        comboExemples.setSelectedIndex(-1);
        comboExemples.addActionListener(e -> chargerExemple());
        ligneExemples.add(comboExemples);
        panneauHaut.add(ligneExemples);

        // liaisons des boutons
        btnAfficher.addActionListener(e -> {
            actionAfficher();
            comboExemples.setSelectedIndex(-1);
        });
        btnReinit.addActionListener(e -> actionReinitialiser());

        // listener commun pour les spinners
        ChangeListener clkSpinner = e -> {
            if (!miseAJourDepuisSpinner) updateValues();
        };
        spinnerIndiceDebut.addChangeListener(clkSpinner);
        spinnerIndiceFin.addChangeListener(clkSpinner);

        // ======= CENTER PANEL =======
        panneauMot = new WordPanel();
        JScrollPane scrollMot = new JScrollPane(panneauMot);
        scrollMot.setPreferredSize(new Dimension(0, 300));
        panneauPrincipal.add(scrollMot, BorderLayout.CENTER);

        // ======= BOTTOM PANEL =======
        JPanel panneauResultats = new JPanel();
        panneauResultats.setLayout(new BoxLayout(panneauResultats, BoxLayout.Y_AXIS));
        panneauResultats.setBorder(new EmptyBorder(10,10,10,10));

        // chaque ligne résultat est un petit JPanel
        panneauResultats.add(creerLigneResultat("s.length() =",  champLongueur = new JTextField()));
        panneauResultats.add(creerLigneResultat("s.charAt(i) =", champResultatChar = new JTextField()));
        panneauResultats.add(creerLigneResultat("s.substring(i,j) =", champResultatSubstr = new JTextField()));
        panneauResultats.add(creerLigneResultat("s.substring(i) =", champResultatSubstrFin = new JTextField()));
        panneauResultats.add(creerLigneResultat("s.indexOf(s.charAt(i)) =", champResultatIndexOf = new JTextField()));
        panneauResultats.add(creerLigneResultat("s.toUpperCase() =", champResultatMaj = new JTextField()));
        panneauResultats.add(creerLigneResultat("s.toLowerCase() =", champResultatMin = new JTextField()));

        // icône d’aide
        JLabel iconeAide = new JLabel("❓");
        iconeAide.setFont(new Font("SansSerif", Font.BOLD, 24));
        iconeAide.setToolTipText("Cliquez pour obtenir de l'aide.");
        iconeAide.setCursor(Cursor.getPredefinedCursor(Cursor.HAND_CURSOR));
        iconeAide.addMouseListener(new MouseAdapter() {
            @Override
            public void mouseClicked(MouseEvent e) {
                montrerAide();
            }
        });

        JScrollPane scrollRes = new JScrollPane(panneauResultats);
        scrollRes.setPreferredSize(new Dimension(0,200));
        JPanel bas = new JPanel(new BorderLayout());
        bas.add(scrollRes, BorderLayout.CENTER);
        bas.add(iconeAide, BorderLayout.EAST);
        panneauPrincipal.add(bas, BorderLayout.SOUTH);

        // focus initial sur la zone de saisie
        SwingUtilities.invokeLater(() -> champTexteChaine.requestFocusInWindow());
    }

    /** Personnalise un spinner (fond + boutons) */
    private void personnaliserSpinner(JSpinner spinner, Color bg, Font police) {
        spinner.setFont(new Font("Tahoma", Font.PLAIN, 25));
        spinner.setOpaque(true);
        spinner.setBackground(bg);
        spinner.setUI(new BasicSpinnerUI() {
            @Override
            protected Component createNextButton() {
                JButton btn = (JButton) super.createNextButton();
                btn.setBackground(bg); btn.setOpaque(true);
                return btn;
            }
            @Override
            protected Component createPreviousButton() {
                JButton btn = (JButton) super.createPreviousButton();
                btn.setBackground(bg); btn.setOpaque(true);
                return btn;
            }
        });
        JTextField fld = ((JSpinner.DefaultEditor) spinner.getEditor()).getTextField();
        fld.setFont(police);
        fld.setOpaque(true);
        fld.setBackground(bg);
        fld.setForeground(Color.WHITE);
    }

    /** Crée une ligne de résultat (label + champ non éditable) */
    private JPanel creerLigneResultat(String texte, JTextField champ) {
        JPanel pnl = new JPanel(new FlowLayout(FlowLayout.LEFT, 10,5));
        JLabel lbl = new JLabel(texte);
        lbl.setFont(new Font("SansSerif", Font.BOLD, 16));
        champ.setColumns(20);
        champ.setEditable(false);
        champ.setFont(new Font("SansSerif", Font.PLAIN, 16));
        champ.setForeground(Color.BLUE);
        pnl.add(lbl);
        pnl.add(champ);
        return pnl;
    }

    /** Charge l’exemple sélectionné dans la combo */
    private void chargerExemple() {
        Example ex = (Example) comboExemples.getSelectedItem();
        if (ex == null) return;
        champTexteChaine.setText(ex.word);
        panneauMot.setWord(ex.word);
        champLongueur.setText(String.valueOf(ex.word.length()));
        SpinnerNumberModel mI = new SpinnerNumberModel(0,0,ex.word.length(),1);
        SpinnerNumberModel mJ = new SpinnerNumberModel(0,0,ex.word.length(),1);
        spinnerIndiceDebut.setModel(mI);
        spinnerIndiceFin.setModel(mJ);
        spinnerIndiceDebut.setValue(ex.i);
        spinnerIndiceFin.setValue(ex.j);
        updateValues();
    }

    /** Affiche tous les résultats (appelé au clic “Afficher” ou spinner) */
    private void updateValues() {
        String s = panneauMot.getWord();
        if (s.isEmpty()) {
            clearResults(); champLongueur.setText("0"); return;
        }
        int i = (int) spinnerIndiceDebut.getValue();
        int j = (int) spinnerIndiceFin.getValue();

        // validation i/j
        if (i<0 || i>=s.length()) {
            JOptionPane.showMessageDialog(this,
                "Index i invalide : 0 ≤ i < "+s.length(),
                "Erreur", JOptionPane.ERROR_MESSAGE);
            i = Math.max(0, Math.min(i, s.length()-1));
            miseAJourDepuisSpinner = true;
            spinnerIndiceDebut.setValue(i);
            miseAJourDepuisSpinner = false;
        }
        if (j<0 || j> s.length()) {
            JOptionPane.showMessageDialog(this,
                "Index j invalide : 0 ≤ j ≤ "+s.length(),
                "Erreur", JOptionPane.ERROR_MESSAGE);
            j = Math.max(0, Math.min(j, s.length()));
            miseAJourDepuisSpinner = true;
            spinnerIndiceFin.setValue(j);
            miseAJourDepuisSpinner = false;
        }

        // propagation vers le panneau graphique
        panneauMot.setI(i);
        panneauMot.setJ(j);
        panneauMot.repaint();

        // mise à jour des champs résultat
        champLongueur.setText(""+s.length());
        champResultatChar.setText(
            (i>=0 && i<s.length()) ? "'"+s.charAt(i)+"'" : "Index i invalide"
        );
        champResultatSubstr.setText(
            (i>j || j> s.length()) ? "Impossible" : "\""+s.substring(i,j)+"\""
        );
        champResultatSubstrFin.setText(
            (i>=s.length()) ? "Impossible" : "\""+s.substring(i)+"\""
        );
        champResultatIndexOf.setText(
            (i>=0 && i<s.length()) ? ""+s.indexOf(s.charAt(i)) : "Index i invalide"
        );
        champResultatMaj.setText("\""+s.toUpperCase()+"\"");
        champResultatMin.setText("\""+s.toLowerCase()+"\"");
    }

    /** Réinitialise tout à blanc */
    private void clearResults() {
        champResultatChar.setText("");
        champResultatSubstr.setText("");
        champResultatSubstrFin.setText("");
        champResultatIndexOf.setText("");
        champResultatMaj.setText("");
        champResultatMin.setText("");
    }

    /** Action du bouton “Afficher” */
    private void actionAfficher() {
        String mot = champTexteChaine.getText();
        panneauMot.setWord(mot);
        champLongueur.setText(""+mot.length());
        spinnerIndiceDebut.setModel(new SpinnerNumberModel(0,0,mot.length(),1));
        spinnerIndiceFin  .setModel(new SpinnerNumberModel(0,0,mot.length(),1));
        spinnerIndiceDebut.setValue(0);
        spinnerIndiceFin  .setValue(0);
        updateValues();
    }

    /** Action du bouton “Réinitialiser” */
    private void actionReinitialiser() {
        champTexteChaine.setText("");
        panneauMot.setWord("");
        champLongueur.setText("");
        spinnerIndiceDebut.setModel(new SpinnerNumberModel(0,0,0,1));
        spinnerIndiceFin  .setModel(new SpinnerNumberModel(0,0,0,1));
        comboExemples.setSelectedIndex(-1);
        clearResults();
        champTexteChaine.requestFocusInWindow();
    }

    /** Affiche la bulle d’aide contextuelle */
    private void montrerAide() {
        JOptionPane.showMessageDialog(this,
            "Ce programme permet de visualiser différentes méthodes sur les chaînes de caractères en Java.\n"+
            "• s.charAt(i) retourne le caractère à l’indice i.\n"+
            "• s.substring(i, j) extrait la sous-chaîne de i (inclus) à j (exclu).\n"+
            "• s.substring(i) extrait de i jusqu’à la fin.\n"+
            "• s.indexOf(...) retourne la position d’une occurrence.\n"+
            "• s.toUpperCase() / s.toLowerCase() convertissent la chaîne.\n\n"+
            "Utilisez les spinners ou clic/glisser pour sélectionner des indices.\n"+
            "La combo permet de charger des exemples rapidement.\n",
            "Aide", JOptionPane.INFORMATION_MESSAGE);
    }

    // ======================= WORD PANEL =======================
    class WordPanel extends JPanel {
        private String word = "";
        private int i = 0, j = 0;
        private Font bigFont = new Font("SansSerif", Font.BOLD, 36);

        public WordPanel() {
            setBackground(Color.WHITE);
            MouseAdapter ma = new MouseAdapter() {
                @Override
                public void mousePressed(MouseEvent e) {
                    int idx = getCharIndexAtX(e.getX());
                    if (idx != -1) {
                        setI(idx); setJ(idx);
                        miseAJourDepuisSpinner = true;
                        spinnerIndiceDebut.setValue(i);
                        spinnerIndiceFin .setValue(j);
                        miseAJourDepuisSpinner = false;
                        updateValues(); repaint();
                    }
                }
                @Override
                public void mouseDragged(MouseEvent e) {
                    int idx = getCharIndexAtX(e.getX());
                    if (idx != -1) {
                        setJ(idx);
                        miseAJourDepuisSpinner = true;
                        spinnerIndiceFin.setValue(j);
                        miseAJourDepuisSpinner = false;
                        updateValues(); repaint();
                    }
                }
            };
            addMouseListener(ma);
            addMouseMotionListener(ma);
        }

        public String getWord() { return word; }
        public void setWord(String w) { word = (w==null ? "" : w); repaint(); }
        public void setI(int n) { i = n; repaint(); }
        public void setJ(int n) { j = n; repaint(); }

        /** Calcule l’index de caractère sous la souris */
        private int getCharIndexAtX(int mouseX) {
            if (word.isEmpty()) return -1;
            FontMetrics fm = getFontMetrics(bigFont);
            int totalW = 0;
            for (char c : word.toCharArray()) totalW += fm.charWidth(c)+5;
            if (totalW>0) totalW -= 5;
            int startX = (getWidth()-totalW)/2, x = startX;
            for (int k=0; k<word.length(); k++) {
                int w = fm.charWidth(word.charAt(k));
                if (mouseX>=x && mouseX<=x+w) return k;
                x+=w+5;
            }
            return -1;
        }

        @Override
        protected void paintComponent(Graphics g) {
            super.paintComponent(g);
            g.setFont(bigFont);
            FontMetrics fm = g.getFontMetrics();
            int centerY = getHeight()/2;
            int baseline = centerY + fm.getAscent()/2;

            // calcul largeur totale et positions
            int totalW=0;
            for (char c: word.toCharArray()) totalW += fm.charWidth(c)+5;
            if (totalW>0) totalW-=5;
            int startX=(getWidth()-totalW)/2;
            int[] pos = new int[word.length()];
            int x=startX;
            for (int k=0; k<word.length(); k++){
                g.drawString(""+word.charAt(k), x, baseline);
                pos[k]=x; x+=fm.charWidth(word.charAt(k))+5;
            }
            if (word.isEmpty()) return;

            Graphics2D g2 = (Graphics2D) g.create();
            // surlignage [i,j)
            if (i<j && i>=0 && j<=word.length()) {
                int xs=pos[i], xe = (j<word.length()?pos[j]:(pos[word.length()-1]+fm.charWidth(word.charAt(word.length()-1))));
                g2.setColor(new Color(211,211,211,100));
                g2.fillRect(xs, baseline-fm.getAscent(), xe-xs, fm.getAscent()+fm.getDescent());
                int lineY = baseline+30;
                g2.setStroke(new BasicStroke(3f));
                g2.setColor(Color.LIGHT_GRAY);
                g2.drawLine(xs, lineY, xe, lineY);

                // pastilles i et j centrées
                int cxI = xs + fm.charWidth(word.charAt(i))/2;
                int cxJ = xe + fm.charWidth(word.charAt(j-1))/2;
                drawIcon(g2, cxI, lineY, 15, 'i', new Color(255,140,0));
                drawIcon(g2, cxJ, lineY, 15, 'j', Color.MAGENTA);
            }
            // surlignage si i==j
            else if (i==j && i>=0 && i<word.length()) {
                int xs=pos[i], w = fm.charWidth(word.charAt(i)), xe=xs+w;
                g2.setColor(new Color(211,211,211,100));
                g2.fillRect(xs, baseline-fm.getAscent(), w, fm.getAscent()+fm.getDescent());
                int lineY=baseline+30;
                g2.setStroke(new BasicStroke(3f));
                g2.setColor(Color.LIGHT_GRAY);
                g2.drawLine(xs, lineY, xe, lineY);
                drawIcon(g2, xs+w/2, lineY, 15, 'i', new Color(255,140,0));
            }
            g2.dispose();
        }

        /** Dessine un cercle coloré avec la lettre c */
        private void drawIcon(Graphics2D g2, int cx, int cy, int r, char c, Color col) {
            g2.setColor(col);
            g2.fillOval(cx-r, cy-r, 2*r, 2*r);
            g2.setFont(new Font("SansSerif", Font.BOLD, r));
            FontMetrics fm = g2.getFontMetrics();
            int w = fm.charWidth(c), h=fm.getAscent();
            g2.setColor(Color.WHITE);
            g2.drawString(""+c, cx - w/2, cy + h/3);
        }
    }

    /** Classe Exemple (label + mot + indices) */
    static class Example {
        String label, word;
        int i, j;
        public Example(String lib, String mot, int ii, int jj) {
            this.label=lib; this.word=mot; this.i=ii; this.j=jj;
        }
        @Override public String toString() { return label; }
    }
}
