import 'package:flutter/material.dart';

void main() {
  runApp(const CaixaEdicaoApp());
}

class CaixaEdicaoApp extends StatelessWidget {
  const CaixaEdicaoApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Exemplo de Entrada de Dados',
      home: const PaginaCaixasEdicao(),
    );
  }
}

// Classe para representar um cliente com nome, email
class Cliente {
  String nome;
  String email;
  String cpf;
  String logradouro;
  String complemento;
  String cidade;
  String bairro;

  Cliente(this.nome, this.email, this.cpf, this.logradouro, this.complemento, this.cidade, this.bairro);
}

class PaginaCaixasEdicao extends StatefulWidget {
  const PaginaCaixasEdicao({Key? key}) : super(key: key);

  @override
  State<PaginaCaixasEdicao> createState() => _PaginaCaixasEdicaoState();
}

class _PaginaCaixasEdicaoState extends State<PaginaCaixasEdicao> {
  // Lista para armazenar os clientes adicionados
  final List<Cliente> _clientes = [];
  
  // Controladores para os campos de entrada
  final TextEditingController _controladorNome = TextEditingController();
  final TextEditingController _controladorEmail = TextEditingController();
  final TextEditingController _controladorCpf = TextEditingController();
  final TextEditingController _controladorLogradouro = TextEditingController();
  final TextEditingController _controladorComplemento = TextEditingController();
  final TextEditingController _controladorCidade = TextEditingController();
  final TextEditingController _controladorBairro = TextEditingController();

  // Índice do cliente em edição
  int _indiceClienteEmEdicao = -1;

  // Método para criar campos de entrada de texto reutilizáveis
  TextField criarCaixaEdicao({
    required TextEditingController controlador,
    required String rotulo,
    required String dica,
    required IconData icon,
  }) {
    return TextField(
      controller: controlador,
      decoration: InputDecoration(
        labelText: rotulo,
        border: OutlineInputBorder(),
        hintText: dica, 
        prefixIcon: Icon(icon),
      ),
    );
  }

  // Método para adicionar um novo cliente à lista de clientes
  void adicionarCliente() {
    setState(() {
      if (_indiceClienteEmEdicao == -1) {
        _clientes.add(Cliente(_controladorNome.text, _controladorEmail.text, _controladorCpf.text, _controladorLogradouro.text, _controladorComplemento.text, _controladorCidade.text, _controladorBairro.text));
      } else {
        _clientes[_indiceClienteEmEdicao] = Cliente(_controladorNome.text, _controladorEmail.text, _controladorCpf.text, _controladorLogradouro.text, _controladorComplemento.text, _controladorCidade.text, _controladorBairro.text);
        _indiceClienteEmEdicao = -1; // Reinicia o índice de cliente em edição
      }
      // Limpa os campos após adicionar ou editar um cliente
      _controladorNome.clear();
      _controladorEmail.clear();
      _controladorCpf.clear();
      _controladorLogradouro.clear();
      _controladorComplemento.clear();
      _controladorCidade.clear();
      _controladorBairro.clear();
    });
  }

  void editarCliente(int index) {
    setState(() {
      // Preenche os campos com as informações do cliente selecionado para edição
      final cliente = _clientes[index];
      _controladorNome.text = cliente.nome;
      _controladorEmail.text = cliente.email;
      _controladorCpf.text = cliente.cpf;
      _controladorLogradouro.text = cliente.logradouro;
      _controladorComplemento.text = cliente.complemento;
      _controladorCidade.text = cliente.cidade;
      _controladorBairro.text = cliente.bairro;
      // Define o índice do cliente em edição
      _indiceClienteEmEdicao = index;
    });
  }

  void excluirCliente(int index) {
    setState(() {
      _clientes.removeAt(index);
    });
  }

  @override
  void dispose() {
    _controladorNome.dispose();
    _controladorEmail.dispose();
    _controladorCpf.dispose();
    _controladorLogradouro.dispose();
    _controladorComplemento.dispose();
    _controladorCidade.dispose();
    _controladorBairro.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Cadastro de Clientes'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(8.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            // Campos de entrada para cliente
            criarCaixaEdicao(
              controlador: _controladorNome,
              rotulo: 'Nome',
              dica: 'Digite o nome',
              icon: Icons.person,
            ),
            SizedBox(
              height: 10,
            ),
            criarCaixaEdicao(
              controlador: _controladorEmail,
              rotulo: 'Email',
              dica: 'Digite o email',
              icon: Icons.email,
            ),
            SizedBox(
              height: 10,
            ),
            criarCaixaEdicao(
              controlador: _controladorCpf,
              rotulo: 'Cpf',
              dica: 'Digite o CPF',
              icon: Icons.badge
            ),
            SizedBox(
              height: 10
            ),
            criarCaixaEdicao(
              controlador: _controladorLogradouro,
              rotulo: 'Logradouro',
              dica: 'Digite o logradouro',
              icon: Icons.location_on
            ),
            SizedBox(
              height: 10
            ),
            criarCaixaEdicao(
              controlador: _controladorComplemento,
              rotulo: 'Complemento',
              dica: 'Digite o complemento',
              icon: Icons.info
            ),
            SizedBox(
              height: 10
            ),
            criarCaixaEdicao(
              controlador: _controladorCidade,
              rotulo: 'Cidade',
              dica: 'Digite a cidade',
              icon: Icons.location_city
            ),
            SizedBox(
              height: 10
            ),
            criarCaixaEdicao(
              controlador: _controladorBairro,
              rotulo: 'Bairro',
              dica: 'Digite o bairro',
              icon: Icons.apartment
            ),
            SizedBox(
              height: 10
            ),
            // Botão para adicionar ou atualizar um cliente
            ElevatedButton(
              onPressed: adicionarCliente,
              child: Text(_indiceClienteEmEdicao == -1 ? 'Adicionar Cliente' : 'Atualizar Cliente'),
            ),
            SizedBox(
              height: 20,
            ),
            // Tabela para exibir os clientes adicionados
            Expanded(
              child: SingleChildScrollView(
                scrollDirection: Axis.horizontal,
                child: DataTable(
                  columns: [
                    DataColumn(label: Text('Nome')),
                    DataColumn(label: Text('Email')),
                    DataColumn(label: Text('CPF')),
                    DataColumn(label: Text('Logradouro')),
                    DataColumn(label: Text('Complemento')),
                    DataColumn(label: Text('Cidade')),
                    DataColumn(label: Text('Bairro')),
                    DataColumn(label: Text('Ações')), // Coluna para as ações dos clientes
                  ],
                  rows: _clientes.map((cliente) {
                    final index = _clientes.indexOf(cliente);
                    return DataRow(cells: [
                      DataCell(Text(cliente.nome)),
                      DataCell(Text(cliente.email)),
                      DataCell(Text(cliente.cpf)),
                      DataCell(Text(cliente.logradouro)),
                      DataCell(Text(cliente.complemento)),
                      DataCell(Text(cliente.cidade)),
                      DataCell(Text(cliente.bairro)),
                      DataCell(Row(
                        children: [
                          IconButton(
                            onPressed: () => editarCliente(index),
                            icon: Icon(Icons.edit),
                          ),
                          IconButton(
                            onPressed: () => excluirCliente(index),
                            icon: Icon(Icons.delete),
                          ),
                        ],
                      )),
                    ]);
                  }).toList(),
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
