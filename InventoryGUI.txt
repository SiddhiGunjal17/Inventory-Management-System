import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.awt.event.*;

public class InventoryGUI {
    private JFrame frame;
    private JTable table;
    private DefaultTableModel model;
    private JTextField nameField, priceField, quantityField, barcodeField;

    public InventoryGUI() {
        frame = new JFrame("Inventory Management System");
        frame.setSize(700, 400);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLayout(new BorderLayout());

        // Table
        model = new DefaultTableModel(new Object[]{"Name", "Price", "Quantity", "Barcode"}, 0);
        table = new JTable(model);
        frame.add(new JScrollPane(table), BorderLayout.CENTER);

        // Form panel
        JPanel formPanel = new JPanel();
        formPanel.setLayout(new GridLayout(2, 5, 5, 5));

        nameField = new JTextField();
        priceField = new JTextField();
        quantityField = new JTextField();
        barcodeField = new JTextField();

        JButton addButton = new JButton("Add Product");
        JButton deleteButton = new JButton("Delete Selected");
        JButton totalButton = new JButton("Total Stock Value");

        formPanel.add(new JLabel("Name:")); formPanel.add(nameField);
        formPanel.add(new JLabel("Price:")); formPanel.add(priceField);
        formPanel.add(new JLabel("Quantity:")); formPanel.add(quantityField);
        formPanel.add(new JLabel("Barcode:")); formPanel.add(barcodeField);
        formPanel.add(addButton); formPanel.add(deleteButton);

        frame.add(formPanel, BorderLayout.NORTH);
        frame.add(totalButton, BorderLayout.SOUTH);

        // Button actions
        addButton.addActionListener(e -> addProduct());
        deleteButton.addActionListener(e -> deleteProduct());
        totalButton.addActionListener(e -> calculateTotal());

        frame.setVisible(true);
    }

    private void addProduct() {
        try {
            String name = nameField.getText();
            double price = Double.parseDouble(priceField.getText());
            int quantity = Integer.parseInt(quantityField.getText());
            String barcode = barcodeField.getText();
            model.addRow(new Object[]{name, price, quantity, barcode});
            nameField.setText(""); priceField.setText(""); quantityField.setText(""); barcodeField.setText("");
        } catch (NumberFormatException ex) {
            JOptionPane.showMessageDialog(frame, "Invalid number format!");
        }
    }

    private void deleteProduct() {
        int selectedRow = table.getSelectedRow();
        if (selectedRow != -1) {
            model.removeRow(selectedRow);
        } else {
            JOptionPane.showMessageDialog(frame, "Select a row to delete.");
        }
    }

    private void calculateTotal() {
        double total = 0;
        for (int i = 0; i < model.getRowCount(); i++) {
            double price = (double) model.getValueAt(i, 1);
            int qty = (int) model.getValueAt(i, 2);
            total += price * qty;
        }
        JOptionPane.showMessageDialog(frame, "Total Stock Value: $" + total);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(InventoryGUI::new);
    }
}
