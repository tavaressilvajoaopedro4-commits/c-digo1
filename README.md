# c-digo1
import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.HBox;
import javafx.stage.Stage;

import java.util.ArrayList;

public class CadastroApp extends Application {

    private final ArrayList<Pessoa> listaPessoas = new ArrayList<>();

    public static class Pessoa {
        private final String nome;
        private final String cpf;
        private final String email;
        private final String telefone;

        public Pessoa(String nome, String cpf, String email, String telefone) {
            this.nome = nome;
            this.cpf = cpf;
            this.email = email;
            this.telefone = telefone;
        }

        @Override
        public String toString() {
            return "Nome: " + nome +
                    "\nCPF: " + cpf +
                    "\nE-mail: " + email +
                    "\nTelefone: " + telefone;
        }
    }

    @Override
    public void start(Stage primaryStage) {

        TextField txtNome = new TextField();
        TextField txtCpf = new TextField();
        TextField txtEmail = new TextField();
        TextField txtTelefone = new TextField();

        txtNome.setPromptText("Digite o nome");
        txtCpf.setPromptText("Digite o CPF");
        txtEmail.setPromptText("Digite o e-mail");
        txtTelefone.setPromptText("Digite o telefone");

        Button btnSalvar = new Button("Salvar");
        Button btnCancelar = new Button("Cancelar");
        Button btnListar = new Button("Listar");

        btnSalvar.setOnAction(e -> {

            String nome = txtNome.getText().trim();
            String cpf = txtCpf.getText().trim();
            String email = txtEmail.getText().trim();
            String telefone = txtTelefone.getText().trim();

            if (nome.isEmpty() || cpf.isEmpty() || email.isEmpty() || telefone.isEmpty()) {

                Alert alerta = new Alert(Alert.AlertType.WARNING);
                alerta.setTitle("Campos obrigatórios");
                alerta.setHeaderText(null);
                alerta.setContentText("Preencha todos os campos!");
                alerta.showAndWait();
                return;
            }

            Pessoa pessoa = new Pessoa(nome, cpf, email, telefone);
            listaPessoas.add(pessoa);

            Alert alerta = new Alert(Alert.AlertType.INFORMATION);
            alerta.setTitle("Sucesso");
            alerta.setHeaderText(null);
            alerta.setContentText("Cadastro salvo com sucesso!");
            alerta.showAndWait();

            limparCampos(txtNome, txtCpf, txtEmail, txtTelefone);
        });

        btnCancelar.setOnAction(e -> limparCampos(
                txtNome, txtCpf, txtEmail, txtTelefone));

        btnListar.setOnAction(e -> {

            if (listaPessoas.isEmpty()) {
                Alert alerta = new Alert(Alert.AlertType.INFORMATION);
                alerta.setTitle("Lista");
                alerta.setHeaderText(null);
                alerta.setContentText("Nenhuma pessoa cadastrada.");
                alerta.showAndWait();
                return;
            }

            StringBuilder lista = new StringBuilder();

            for (Pessoa p : listaPessoas) {
                lista.append(p)
                     .append("\n\n");
            }

            Alert alerta = new Alert(Alert.AlertType.INFORMATION);
            alerta.setTitle("Pessoas Cadastradas");
            alerta.setHeaderText(null);
            alerta.setContentText(lista.toString());
            alerta.showAndWait();
        });

        GridPane grid = new GridPane();
        grid.setAlignment(Pos.CENTER);
        grid.setPadding(new Insets(20));
        grid.setHgap(10);
        grid.setVgap(10);

        grid.add(new Label("Nome:"), 0, 0);
        grid.add(txtNome, 1, 0);

        grid.add(new Label("CPF:"), 0, 1);
        grid.add(txtCpf, 1, 1);

        grid.add(new Label("E-mail:"), 0, 2);
        grid.add(txtEmail, 1, 2);

        grid.add(new Label("Telefone:"), 0, 3);
        grid.add(txtTelefone, 1, 3);

        HBox botoes = new HBox(10, btnSalvar, btnCancelar, btnListar);
        botoes.setAlignment(Pos.CENTER_RIGHT);

        grid.add(botoes, 1, 4);

        Scene scene = new Scene(grid, 450, 250);

        primaryStage.setTitle("Cadastro de Pessoas");
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    private void limparCampos(TextField nome,
                              TextField cpf,
                              TextField email,
                              TextField telefone) {

        nome.clear();
        cpf.clear();
        email.clear();
        telefone.clear();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
