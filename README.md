import java.util.Scanner;
import java.util.TreeMap;
import java.util.TreeSet;

public class FeiraLivros {
    private static TreeSet<Livro> livros = new TreeSet<>();
    private static TreeMap<String, TreeSet<String>> autores = new TreeMap<>();

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int opcao;

        do {
            System.out.println("\n=== Feira de Livros Usados ===");
            System.out.println("1 - Cadastrar Livro");
            System.out.println("2 - Exibir Todos os Livros");
            System.out.println("3 - Exibir Autores e seus Livros");
            System.out.println("0 - Sair");
            System.out.print("Escolha uma opção: ");
            opcao = scanner.nextInt();
            scanner.nextLine();

            switch (opcao) {
                case 1:
                    cadastrarLivro(scanner);
                    break;
                case 2:
                    exibirLivros();
                    break;
                case 3:
                    exibirAutores();
                    break;
                case 0:
                    System.out.println("Encerrando o programa.");
                    break;
                default:
                    System.out.println("Opção inválida. Tente novamente.");
            }

        } while (opcao != 0);

        scanner.close();
    }

    private static void cadastrarLivro(Scanner scanner) {
        System.out.print("Título do livro: ");
        String titulo = scanner.nextLine();
        System.out.print("Autor do livro: ");
        String autor = scanner.nextLine();
        System.out.print("Ano de publicação: ");
        int ano = scanner.nextInt();
        scanner.nextLine();

        Livro livro = new Livro(titulo, autor, ano);
        livros.add(livro);

        autores.putIfAbsent(autor, new TreeSet<>());
        autores.get(autor).add(titulo);

        System.out.println("Livro cadastrado com sucesso!");
    }

    private static void exibirLivros() {
        System.out.println("\nTodos os livros cadastrados:");
        for (Livro livro : livros) {
            System.out.println(livro);
        }
    }

    private static void exibirAutores() {
        System.out.println("\nAutores e seus livros:");
        for (String autor : autores.keySet()) {
            System.out.println(autor + ":");
            for (String titulo : autores.get(autor)) {
                System.out.println("- " + titulo);
            }
        }
    }
}

class Livro implements Comparable<Livro> {
    private String titulo;
    private String autor;
    private int ano;

    public Livro(String titulo, String autor, int ano) {
        this.titulo = titulo;
        this.autor = autor;
        this.ano = ano;
    }

    public String getTitulo() {
        return titulo;
    }

    @Override
    public int compareTo(Livro outro) {
        return this.titulo.compareToIgnoreCase(outro.titulo);
    }

    @Override
    public String toString() {
        return titulo + " (" + ano + ")";
    }

    @Override
    public boolean equals(Object obj) {
        if (obj instanceof Livro) {
            Livro outro = (Livro) obj;
            return this.titulo.equalsIgnoreCase(outro.titulo);
        }
        return false;
    }

    @Override
    public int hashCode() {
        return titulo.toLowerCase().hashCode();
    }
}
