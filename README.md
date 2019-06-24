//VERIFICAÇÃO DE CNPJ VÁLIDO!
//Adicionando a classe ao botão "Validar". -----------------------------------------------------
 
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace ValidadorCnpj
{
    public class Validacao()
    {
        public bool validarCnpj(string cnpj)
        {
            int[] multiplicador1 = new int[12] { 5, 4, 3, 2, 9, 8, 7, 6, 5, 4, 3, 2 };
            int[] multiplicador2 = new int[13] { 6, 5, 4, 3, 2, 9, 8, 7, 6, 5, 4, 3, 2 };
            int somador;
            int resto;
            string digito;
            string cnpjAux;
            cnpj = cnpj.Trim()
            cnpj = cnpj.Replace(",", "").Replace("/", "").Replace("-", "");
            if (cnpj.Length != 14)
            {
                return False;
            }
            else
            {
                cnpjAux = cnpj.Substring(0, 12);
                somador = 0;
                for (int i = 0; i < 12; i++)
                {
                    somador += int.Parse(cnpjAux[i].ToString()) * multiplicador1[i]);
                }

                resto = (somador % 11);
                if (resto < 0)
                    resto = 0;
                else
                    resto = 11 - resto;
                digito = resto.ToString();
                cnpjAux = cnpjAux + digito;
                somador = 0;
                for (int i = 0; i < 13; i++)
                {
                    somador += int.Parse(cnpjAux[i].ToString()) * multiplicador2[i]);
                }

                resto = (somador % 11);
                if (resto< 0)
                    resto = 0;
                else
                    resto = 11 - resto;
                digito = digito + resto.ToString();
                return  cnpj.EndsWith(digito)
            }
        }
    }  

//Usando a classe para o botão validar.-----------------------------------------------------

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WindowsFormsApp2
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void maskedTextBox1_MaskInputRejected(object sender, MaskInputRejectedEventArgs e)
        {

        }

        private void btnValidaCnpj_Click(object sender, EventArgs e)
        {
            Validacao valid = new Validacao();
            mtbCnpj.TextMaskFormat = MaskFormat.IncludeLiterals;
            string cnpj = mtbCnpj.Text;
            bool verFal = valid.validarCnpj(cnpj);
            if (verFal)
                MessageBox.Show("CNPJ Válido!");
            else
                MessageBox.Show("CNPJ Inválido!");
        }
    }
}

//Cadastrando Cnpj ao Banco de Dados.  -----------------------------------------------------

using System;
using System.Collections.Generic;
using System.Data.SqlClient;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsultaCnpj
{
    public class Cadastro
    {
        ConsultaCnpj = new ConsultaCnpj();
        SqlCommand cmd = new SqlCommand();
        public String mensagem = "";

        public Cadastro(string cnpj)
        {
            cmd.CommandText = "insert into empresa (cnpj) values (@cnpj)";//Comando do SqlServer

            cmd.Parameters.AddWithValue("@cnpj", cnpj);//Parametros

            try
            {
                cmd.Connection = ConsultaCnpj.conectar();
                cmd.ExecuteNonQuery();
                ConsultaCnpj.desconectar();
                this.mensagem = "Cadastrado com Sucesso!";
            }
            catch (SqlException e)
            {
                this.mensagem = "Erro ao tentar se conectar com o Banco de Dados.";
            }
        }
    }
}

//Conectar ao Banco de dados -----------------------------------------------------

using System;
using System.Collections.Generic;
using System.Data.SqlClient;
using System.Linq;
using System.Runtime.Remoting.Messaging;
using System.Text;
using System.Threading.Tasks;

namespace ConsultaCnpj
{
    public class ConsultarCnpj
    {
        SqlConnection con = new SqlConnection();

        public ConsultaCnpj()
        {
            con.ConnectionString = "/* Endereço do Banco de Dados*/";

        }
        
        public SqlConnection conectar() //Se conecta com o Banco de Dados
        {
            if(con.State == System.Data.ConnectionState.Closed)
                con.Open();

        }

        return con;

        public void desconectar()
        {
            if (con.State == System.Data.ConnectionState.Open)
                con.Close();
        }
    }
}

//Depois que cadastrou o Cnpj ----------------------------------------------------

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
//using sqlserver.data sqlserverclient; Puxando o conteúdo do banco de dados (simular)

namespace ConsultaCnpj
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }
        
        private void btnBuscar_Click(object sender, EventArgs e)
        {
            try
            {
                
            }
            catch (Exception erro)
            {
                MessageBox.Show("Erro ao buscar" + erro);
            }
        }

        private void Form1_Load(object sender, EventArgs e)
        {

        }
        
        private void btnInserir_Click(object sender, EventArgs e)//Botão Cadastrar
        {
            Cadastro cad = new Cadastro(mtbCnpj);
            MessageBox.Show(cad.mensagem);
        }
    }
}

//Com insert e delete (Outra tentativa)----------------------------------------------------
using System;
using System.Collections.Generic;
using System.Data.SqlClient;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Data.SqlClient; //Usando Banco de Dados

namespace ConsultaCnpj
{
    public class Cadastro
    {
        SqlConnection conn = new SqlConnection("Data Source=NOTEBOOK-JNA5008\\SQLEXPRESS;Initial Catalog=info;Integrated Security=True");
        SqlCommand comando = new SqlCommand();
        SqlDataReader dr;
    }
    public Form1()
    {
        InitializeComponent();
    }
    private void Form1_Load(object sender, EventArgs e)
    {
        comando.Connection = conn;
        carregarLista();
    }
    Private void button1_Click(object sender, EventArgs e)
    {
        if(MaskedTextBox1 != "")
        {
            conn.Open();
            comando.CommandText = "insert into empresas(cnpj) values ('"+MaskedTextBox1+"')";
            comando.ExecuteNonQuery();
            conn.Close();

            carregarLista();

            MaskedTextBox1.Text = "";

            private void carregarLista();

            ListBox1.Items.Clear();

            conn.Open();
            comando.CommandText = "select *from empresas";
            dr = comando.ExecuteReader();
        }
            if(dr.HasRows)
                {
                    while (dr.Read)
                        {
                            ListBox1.Items.Add(dr[0].ToString());
                        }
                }
             conn.Close();
}
    private void button2_Click(object sender, EventArgs e)
    {
        if (MaskedTextBox1 != "")
        {
            conn.Open();
            comando.CommandText = "delete from empresa WHERE cnpj = "'+MaskedTextBox1+'";
            comando.ExecuteNonQuery();
            conn.Close();

            carregarLista();

            MaskedTextBox1.Text = "";

            private void carregarLista();

            ListBox1.Items.Clear();

            conn.Open();
            comando.CommandText = "select *from empresas";
            dr = comando.ExecuteReader();
        }
    }
}
