un banco requiere un sistema que le permita registrar, actualizar, vender y eliminar creditos de usuarios 
por los cuales requiere que el departamento de TI apoye con este tema
para vender creditos se deben tener en cuenta lo siguiente 

1) que el usuario ya este registrado en caso que no. que no muestre la opcion de registro sino que muestre la opcion del ingreso de registros es decir enlace directo
2) al vender o solicitar credito se debe validar el tipo de credito si no son iguales debe hacerse otro credito diferente.
public class Credito {
    private String usuario;
    private String tipoCredito;
    private double monto;

    public Credito(String usuario, String tipoCredito, double monto) {
        this.usuario = usuario;
        this.tipoCredito = tipoCredito;
        this.monto = monto;
    }

    public String getUsuario() {
        return usuario;
    }

    public String getTipoCredito() {
        return tipoCredito;
    }

    public double getMonto() {
        return monto;
    }

    public void setMonto(double monto) {
        this.monto = monto;
    }

    @Override
    public String toString() {
        return "Usuario: " + usuario + ", Tipo: " + tipoCredito + ", Monto: $" + monto;
    }
}


import java.util.Stack;
import javax.swing.JOptionPane;

public class Banco {
    private Stack<Credito> creditos = new Stack<>();

    // Menú principal
    public void mostrarMenu() {
        int opcion;

        do {
            opcion = Integer.parseInt(JOptionPane.showInputDialog(
                "Menú del Banco\n" +
                "1. Registrar crédito\n" +
                "2. Mostrar créditos\n" +
                "3. Vender crédito\n" +
                "4. Eliminar crédito\n" +
                "5. Salir\n" +
                "Seleccione una opción:"));

            switch (opcion) {
                case 1:
                    registrarCredito();
                    break;
                case 2:
                    mostrarCreditos();
                    break;
                case 3:
                    venderCredito();
                    break;
                case 4:
                    eliminarCredito();
                    break;
                case 5:
                    JOptionPane.showMessageDialog(null, "Saliendo del sistema...");
                    break;
                default:
                    JOptionPane.showMessageDialog(null, "Opción inválida. Intente de nuevo.");
            }
        } while (opcion != 5);
    }

    // Registrar un nuevo crédito
    public void registrarCredito() {
        String usuario = JOptionPane.showInputDialog("Ingrese el nombre del usuario:");
        String tipo = JOptionPane.showInputDialog("Ingrese el tipo de crédito:");
        double monto = Double.parseDouble(JOptionPane.showInputDialog("Ingrese el monto del crédito:"));

        creditos.push(new Credito(usuario, tipo, monto));
        JOptionPane.showMessageDialog(null, "Crédito registrado con éxito.");
    }

    // Mostrar créditos
    public void mostrarCreditos() {
        if (creditos.isEmpty()) {
            JOptionPane.showMessageDialog(null, "No hay créditos registrados.");
            return;
        }

        StringBuilder lista = new StringBuilder("Créditos Registrados:\n");
        for (Credito c : creditos) {
            lista.append(c.toString()).append("\n");
        }

        JOptionPane.showMessageDialog(null, lista.toString());
    }

    // Vender un crédito
    public void venderCredito() {
        if (creditos.isEmpty()) {
            JOptionPane.showMessageDialog(null, "No hay créditos disponibles.");
            return;
        }

        String usuario = JOptionPane.showInputDialog("Ingrese el usuario que solicita el crédito:");
        String tipo = JOptionPane.showInputDialog("Ingrese el tipo de crédito que solicita:");
        double monto = Double.parseDouble(JOptionPane.showInputDialog("Ingrese el monto solicitado:"));

        Stack<Credito> aux = new Stack<>();
        boolean encontrado = false;

        while (!creditos.isEmpty()) {
            Credito c = creditos.pop();
            if (c.getUsuario().equalsIgnoreCase(usuario)) {
                if (c.getTipoCredito().equalsIgnoreCase(tipo)) {
                    c.setMonto(c.getMonto() + monto);
                    JOptionPane.showMessageDialog(null, "Crédito actualizado con éxito.");
                } else {
                    JOptionPane.showMessageDialog(null, "El usuario tiene un crédito de tipo diferente. Se creará un nuevo crédito.");
                    aux.push(c);
                    creditos.push(new Credito(usuario, tipo, monto));
                }
                encontrado = true;
                aux.push(c);
                break;
            } else {
                aux.push(c);
            }
        }

        while (!aux.isEmpty()) {
            creditos.push(aux.pop());
        }

        if (!encontrado) {
            JOptionPane.showMessageDialog(null, "Usuario no encontrado.");
        }
    }

    // Eliminar un crédito
    public void eliminarCredito() {
        if (creditos.isEmpty()) {
            JOptionPane.showMessageDialog(null, "No hay créditos para eliminar.");
            return;
        }

        String usuario = JOptionPane.showInputDialog("Ingrese el usuario cuyo crédito desea eliminar:");
        Stack<Credito> aux = new Stack<>();
        boolean eliminado = false;

        while (!creditos.isEmpty()) {
            Credito c = creditos.pop();
            if (!c.getUsuario().equalsIgnoreCase(usuario)) {
                aux.push(c);
            } else {
                eliminado = true;
            }
        }

        while (!aux.isEmpty()) {
            creditos.push(aux.pop());
        }

        if (eliminado) {
            JOptionPane.showMessageDialog(null, "Crédito eliminado con éxito.");
        } else {
            JOptionPane.showMessageDialog(null, "Usuario no encontrado.");
        }
    }
}
public class Main {
    public static void main(String[] args) {
        Banco banco = new Banco();
        banco.mostrarMenu();
    }
}






