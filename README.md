# kve
# первая форма (какая-то авторизация):
private Dictionary<string, (string password, UserRole role)> users = new Dictionary<string, (string, UserRole)>
{
    { "admin", ("admin123", UserRole.Admin) },
    { "client", ("client123", UserRole.Client) }
};
private void button1_Click(object sender, EventArgs e)
{
    string username = textBox1.Text.Trim();
    string password = textBox2.Text;
    if (string.IsNullOrEmpty(username) || string.IsNullOrEmpty(password))
    {
        MessageBox.Show("Введите логин и пароль", "Ошибка",
        MessageBoxButtons.OK, MessageBoxIcon.Warning);
        return;
    }
    if (users.ContainsKey(username))
    {
        var userData = users[username];
        if (userData.password == password)
        {
            UserSession.Username = username;
            UserSession.Role = userData.role;
            Form1 form = new Form1();
            this.Hide();
            form.ShowDialog();
            this.Close();
        }
        else
        {
            MessageBox.Show("Неверный пароль", "Ошибка",
            MessageBoxButtons.OK, MessageBoxIcon.Error);
        }
    }
    else
    {
        MessageBox.Show("Пользователь не найден", "Ошибка",
        MessageBoxButtons.OK, MessageBoxIcon.Error);
    }
}
public enum UserRole
{
    Admin,
    Client
}
public static class UserSession
{
    public static UserRole Role { get; set; }
    public static string Username { get; set; }
}
# вторая форма (какой-то sql запрос):
String connection = "Host=localhost;Port=5432;Database=airport;Username=postgres;Password=1";
public Form2()
{
    InitializeComponent();
    LoadData();
}
void LoadData()
{
    try
    {
        using (NpgsqlConnection conn = new NpgsqlConnection(connection))
        {
            conn.Open();
            string sql = "SELECT * FROM public.\"table\"";
            using (NpgsqlCommand cmd = new NpgsqlCommand(sql, conn))
            {
                DataTable dt = new DataTable();
                using (NpgsqlDataAdapter da = new NpgsqlDataAdapter(cmd))
                {
                    da.Fill(dt);
                }
                dataGridView1.DataSource = dt;
            }
        }
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Ошибка: {ex.Message}");
    }
}
# переход с одной формы на другую (пример):
private void button1_Click(object sender, EventArgs e)
{
    Form2 form = new Form2();
    this.Hide();
    form.ShowDialog();
    this.Close();
}
