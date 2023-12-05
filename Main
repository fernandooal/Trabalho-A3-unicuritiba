package br.com.unicuritiba;

import javax.swing.*;
import javax.swing.border.EmptyBorder;
import java.awt.*;
import java.io.File;
import java.util.ArrayList;
import java.util.List;

public class Main extends JFrame {

    private JTextField directoryField;
    private JTextField filesField;
    private JTextArea resultArea;
    private JLabel statusLabel;
    private ImageIcon loadingIcon;

    public Main() {
        super("Antivirus");

        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(800, 600);
        setLocationRelativeTo(null);

        setLayout(new BorderLayout());

        loadingIcon = new ImageIcon("loading.gif");

        JPanel mainPanel = new JPanel(new BorderLayout());
        mainPanel.setBackground(new Color(240, 240, 255));

        JPanel inputPanel = new JPanel(new GridLayout(3, 2, 10, 10));
        inputPanel.setBorder(new EmptyBorder(10, 10, 10, 10));

        inputPanel.add(new JLabel("Diretório Raiz:"));
        directoryField = new JTextField();
        inputPanel.add(directoryField);

        inputPanel.add(new JLabel("Arquivos (separados por vírgula):"));
        filesField = new JTextField();
        inputPanel.add(filesField);

        JButton searchButton = new JButton("Buscar");
        searchButton.addActionListener(e -> onSearchButtonClick());

        JButton deleteButton = new JButton("Excluir");
        deleteButton.addActionListener(e -> onDeleteButtonClick());

        JPanel buttonPanel = new JPanel(new FlowLayout(FlowLayout.RIGHT, 10, 10));
        buttonPanel.add(searchButton);
        buttonPanel.add(deleteButton);

        inputPanel.add(buttonPanel);

        mainPanel.add(inputPanel, BorderLayout.NORTH);

        resultArea = new JTextArea();
        resultArea.setEditable(false);
        resultArea.setFont(new Font("Monospaced", Font.PLAIN, 12));

        statusLabel = new JLabel("Insira os dados e clique em Buscar.", loadingIcon, SwingConstants.LEADING);
        statusLabel.setBorder(new EmptyBorder(5, 10, 5, 10));

        mainPanel.add(new JScrollPane(resultArea), BorderLayout.CENTER);
        mainPanel.add(statusLabel, BorderLayout.SOUTH);

        add(mainPanel);

        resultArea.setBackground(new Color(255, 255, 240));
    }

    private void onSearchButtonClick() {
        String directory = directoryField.getText();
        String filesInput = filesField.getText();
        List<String> filesToSearch = parseFilesInput(filesInput);

        statusLabel.setText("Verificando arquivos...");

        SwingWorker<Void, Void> worker = new SwingWorker<Void, Void>() {
            @Override
            protected Void doInBackground() {
                List<String> foundFiles = buscarArquivos(directory, filesToSearch);

                resultArea.setText("");
                if (foundFiles.isEmpty()) {
                    resultArea.append("Nenhum arquivo encontrado.");
                } else {
                    resultArea.append("Arquivos encontrados:\n");
                    for (String path : foundFiles) {
                        resultArea.append(path + "\n");
                    }
                }

                return null;
            }

            @Override
            protected void done() {
                statusLabel.setIcon(null);
                statusLabel.setText("Busca concluída.");
            }
        };

        worker.execute();
    }

    private void onDeleteButtonClick() {
        String selectedPath = resultArea.getSelectedText();

        if (selectedPath != null && !selectedPath.isEmpty()) {
            int option = JOptionPane.showConfirmDialog(this, "Tem certeza que deseja excluir o arquivo selecionado?", "Confirmar Exclusão", JOptionPane.YES_NO_OPTION);

            if (option == JOptionPane.YES_OPTION) {
                File fileToDelete = new File(selectedPath);
                if (fileToDelete.delete()) {
                    JOptionPane.showMessageDialog(this, "Arquivo excluído com sucesso!");
                    resultArea.setText("");
                } else {
                    JOptionPane.showMessageDialog(this, "Falha ao excluir o arquivo.", "Erro", JOptionPane.ERROR_MESSAGE);
                }
            }
        } else {
            JOptionPane.showMessageDialog(this, "Nenhum arquivo selecionado para exclusão.", "Aviso", JOptionPane.WARNING_MESSAGE);
        }
    }

    private List<String> parseFilesInput(String filesInput) {
        String[] filesArray = filesInput.split(",");
        List<String> filesList = new ArrayList<>();
        for (String file : filesArray) {
            filesList.add(file.trim());
        }
        return filesList;
    }

    private List<String> buscarArquivos(String directory, List<String> filesToSearch) {
        List<String> foundFiles = new ArrayList<>();
        File rootDir = new File(directory);

        for (String fileName : filesToSearch) {
            buscarArquivoRecursivo(rootDir, fileName, foundFiles);
        }

        return foundFiles;
    }

    private void buscarArquivoRecursivo(File directory, String fileName, List<String> foundFiles) {
        File[] files = directory.listFiles();

        if (files != null) {
            for (File file : files) {
                if (file.isFile() && file.getName().equalsIgnoreCase(fileName)) {
                    foundFiles.add(file.getAbsolutePath());
                } else if (file.isDirectory()) {
                    buscarArquivoRecursivo(file, fileName, foundFiles);
                }
            }
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            try {
                UIManager.setLookAndFeel(UIManager.getSystemLookAndFeelClassName());
            } catch (Exception e) {
                e.printStackTrace();
            }
            new Main().setVisible(true);
        });
    }
}
