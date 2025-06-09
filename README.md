import java.util.*;

public class CalculadoraPosOrdem {

    // Método principal que realiza o cálculo da expressão em notação pós-fixada
    public static double calcular(String expressao) throws Exception {
        Queue<String> fila = new LinkedList<>(); // Fila para armazenar a expressão
        Stack<Double> pilha = new Stack<>();      // Pilha para calcular o resultado

        // Adiciona cada elemento da expressão na fila
        for (String token : expressao.trim().split("\\s+")) {
            fila.add(token);
        }

        // Processa cada elemento da fila
        while (!fila.isEmpty()) {
            String token = fila.poll(); // Remove o próximo item da fila

            // Se o token for número, empilha
            if (token.matches("-?\\d+(\\.\\d+)?")) {
                pilha.push(Double.parseDouble(token));
            }
            // Se for operador, realiza operação com os dois últimos números da pilha
            else if ("+-*/%".contains(token)) {
                if (pilha.size() < 2) throw new Exception("Erro: operandos insuficientes.");
                double b = pilha.pop();
                double a = pilha.pop();

                switch (token) {
                    case "+": pilha.push(a + b); break;
                    case "-": pilha.push(a - b); break;
                    case "*": pilha.push(a * b); break;
                    case "/":
                        if (b == 0) throw new ArithmeticException("Erro: divisão por zero.");
                        pilha.push(a / b);
                        break;
                    case "%":
                        if (b == 0) throw new ArithmeticException("Erro: divisão por zero.");
                        pilha.push(a % b);
                        break;
                    default:
                        throw new Exception("Erro: operador inválido.");
                }
            }
            // Caso o token não seja número nem operador
            else {
                throw new Exception("Erro: token inválido -> " + token);
            }
        }

        // Verifica se há exatamente um resultado na pilha
        if (pilha.size() != 1) throw new Exception("Erro: expressão malformada.");
        return pilha.pop(); // Retorna o resultado final
    }

    // Método principal para entrada do usuário
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Digite a expressão pós-fixada (ex: 3 4 +):");
        String expressao = scanner.nextLine();

        try {
            double resultado = calcular(expressao); // Chama o método de cálculo
            System.out.println("Resultado: " + resultado);
        } catch (Exception e) {
            System.out.println(e.getMessage()); // Mostra mensagens de erro
        }
    }
}
