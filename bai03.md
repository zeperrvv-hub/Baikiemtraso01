using System;
using System.ComponentModel;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Text;
using System.Windows.Forms;

namespace bai03
{
    public class Form1 : Form
    {
        private BindingList<Product> products =
            new BindingList<Product>();

        private BindingSource bindingSource =
            new BindingSource();

        private TextBox txtProductId;
        private TextBox txtProductName;
        private TextBox txtUnitPrice;
        private TextBox txtQuantity;
        private TextBox txtSearch;

        private ComboBox cboCategory;

        private PictureBox picAvatar;

        private Button btnChooseImage;
        private Button btnAdd;
        private Button btnUpdate;
        private Button btnDelete;

        private DataGridView dgvProducts;

        private ErrorProvider errorProvider;

        private ToolStripStatusLabel statusLabel;

        public Form1()
        {
            CreateInterface();
            LoadData();
            RegisterEvents();
        }

        private void CreateInterface()
        {
            Text = "TechMart Product Manager";

            StartPosition =
                FormStartPosition.CenterScreen;

            Size = new Size(1200, 700);

            MinimumSize =
                new Size(900, 550);

    
            MenuStrip menuStrip =
                new MenuStrip();

            ToolStripMenuItem fileMenu =
                new ToolStripMenuItem("File");

            ToolStripMenuItem exportMenu =
                new ToolStripMenuItem("Export CSV");

            exportMenu.ShortcutKeys =
                Keys.Control | Keys.E;

            exportMenu.Click += ExportCsv_Click;

            ToolStripMenuItem exitMenu =
                new ToolStripMenuItem("Exit");

            exitMenu.ShortcutKeys =
                Keys.Control | Keys.X;

            exitMenu.Click += Exit_Click;

            fileMenu.DropDownItems.Add(exportMenu);
            fileMenu.DropDownItems.Add(exitMenu);

            menuStrip.Items.Add(fileMenu);

            menuStrip.Dock =
                DockStyle.Top;

            Controls.Add(menuStrip);

            StatusStrip statusStrip =
                new StatusStrip();

            statusLabel =
                new ToolStripStatusLabel();

            statusLabel.Text =
                "Tổng số sản phẩm: 0";

            statusStrip.Items.Add(statusLabel);

            statusStrip.Dock =
                DockStyle.Bottom;

            Controls.Add(statusStrip);

        
            TableLayoutPanel mainTable =
                new TableLayoutPanel();

            mainTable.Dock =
                DockStyle.Fill;

            mainTable.ColumnCount = 2;
            mainTable.RowCount = 1;

           
            mainTable.ColumnStyles.Add(
                new ColumnStyle(
                    SizeType.Percent,
                    35F));

         
            mainTable.ColumnStyles.Add(
                new ColumnStyle(
                    SizeType.Percent,
                    65F));

            Controls.Add(mainTable);

            TableLayoutPanel leftTable =
                new TableLayoutPanel();

            leftTable.Dock =
                DockStyle.Fill;

            leftTable.Padding =
                new Padding(10);

            leftTable.ColumnCount = 2;

            leftTable.ColumnStyles.Add(
                new ColumnStyle(
                    SizeType.Percent,
                    35F));

            leftTable.ColumnStyles.Add(
                new ColumnStyle(
                    SizeType.Percent,
                    65F));

            mainTable.Controls.Add(
                leftTable,
                0,
                0);

           
            AddTextBox(
                leftTable,
                "Mã SP:",
                out txtProductId);

           
            AddTextBox(
                leftTable,
                "Tên SP:",
                out txtProductName);

            
            Label lblCategory =
                CreateLabel("Danh mục:");

            cboCategory =
                new ComboBox();

            cboCategory.Dock =
                DockStyle.Fill;

            cboCategory.DropDownStyle =
                ComboBoxStyle.DropDownList;

            AddRow(
                leftTable,
                lblCategory,
                cboCategory);

            AddTextBox(
                leftTable,
                "Đơn giá:",
                out txtUnitPrice);

            AddTextBox(
                leftTable,
                "Số lượng:",
                out txtQuantity);

            Label lblImage =
                CreateLabel("Ảnh:");

            picAvatar =
                new PictureBox();

            picAvatar.Dock =
                DockStyle.Fill;

            picAvatar.Height = 150;

            picAvatar.SizeMode =
                PictureBoxSizeMode.Zoom;

            picAvatar.BorderStyle =
                BorderStyle.FixedSingle;

            AddRow(
                leftTable,
                lblImage,
                picAvatar);

       
            btnChooseImage =
                new Button();

            btnChooseImage.Text =
                "Chọn Ảnh";

            btnChooseImage.AutoSize =
                true;

            btnChooseImage.Click +=
                ChooseImage_Click;

            leftTable.Controls.Add(
                btnChooseImage,
                1,
                leftTable.RowCount - 1);

        
            FlowLayoutPanel buttonPanel =
                new FlowLayoutPanel();

            buttonPanel.Dock =
                DockStyle.Fill;

            btnAdd =
                new Button();

            btnAdd.Text =
                "Thêm mới";

            btnAdd.AutoSize =
                true;

            btnAdd.Click +=
                Add_Click;

            btnUpdate =
                new Button();

            btnUpdate.Text =
                "Cập nhật";

            btnUpdate.AutoSize =
                true;

            btnUpdate.Click +=
                Update_Click;

            btnDelete =
                new Button();

            btnDelete.Text =
                "Xóa";

            btnDelete.AutoSize =
                true;

            btnDelete.Click +=
                Delete_Click;

            buttonPanel.Controls.Add(btnAdd);
            buttonPanel.Controls.Add(btnUpdate);
            buttonPanel.Controls.Add(btnDelete);

            leftTable.Controls.Add(
                buttonPanel,
                0,
                leftTable.RowCount);

            leftTable.SetColumnSpan(
                buttonPanel,
                2);

            leftTable.RowCount++;

       
            TableLayoutPanel rightTable =
                new TableLayoutPanel();

            rightTable.Dock =
                DockStyle.Fill;

            rightTable.Padding =
                new Padding(10);

            rightTable.ColumnCount = 1;
            rightTable.RowCount = 2;

            rightTable.RowStyles.Add(
                new RowStyle(
                    SizeType.Absolute,
                    40F));

            rightTable.RowStyles.Add(
                new RowStyle(
                    SizeType.Percent,
                    100F));

            mainTable.Controls.Add(
                rightTable,
                1,
                0);

     
            FlowLayoutPanel searchPanel =
                new FlowLayoutPanel();

            searchPanel.Dock =
                DockStyle.Fill;

            Label lblSearch =
                CreateLabel("Tìm kiếm:");

            txtSearch =
                new TextBox();

            txtSearch.Width = 300;

            searchPanel.Controls.Add(
                lblSearch);

            searchPanel.Controls.Add(
                txtSearch);

            rightTable.Controls.Add(
                searchPanel,
                0,
                0);

            dgvProducts =
                new DataGridView();

            dgvProducts.Dock =
                DockStyle.Fill;

            dgvProducts.AutoGenerateColumns =
                false;

            dgvProducts.SelectionMode =
                DataGridViewSelectionMode.FullRowSelect;

            dgvProducts.MultiSelect = false;

            dgvProducts.ReadOnly = true;

            dgvProducts.AllowUserToAddRows =
                false;

            dgvProducts.AutoSizeColumnsMode =
                DataGridViewAutoSizeColumnsMode.Fill;

           
            DataGridViewTextBoxColumn colId =
                new DataGridViewTextBoxColumn();

            colId.HeaderText =
                "Mã SP";

            colId.DataPropertyName =
                "ProductId";

          
            DataGridViewTextBoxColumn colName =
                new DataGridViewTextBoxColumn();

            colName.HeaderText =
                "Tên SP";

            colName.DataPropertyName =
                "ProductName";

           
            DataGridViewTextBoxColumn colCategory =
                new DataGridViewTextBoxColumn();

            colCategory.HeaderText =
                "Danh Mục";

            colCategory.DataPropertyName =
                "Category";

          
            DataGridViewTextBoxColumn colPrice =
                new DataGridViewTextBoxColumn();

            colPrice.HeaderText =
                "Đơn Giá";

            colPrice.DataPropertyName =
                "UnitPrice";

            colPrice.DefaultCellStyle.Format =
                "N0";

           
            DataGridViewTextBoxColumn colQuantity =
                new DataGridViewTextBoxColumn();

            colQuantity.HeaderText =
                "Số Lượng";

            colQuantity.DataPropertyName =
                "Quantity";

            dgvProducts.Columns.Add(colId);
            dgvProducts.Columns.Add(colName);
            dgvProducts.Columns.Add(colCategory);
            dgvProducts.Columns.Add(colPrice);
            dgvProducts.Columns.Add(colQuantity);

            rightTable.Controls.Add(
                dgvProducts,
                0,
                1);

          
            errorProvider =
                new ErrorProvider();
        }

    
        private void LoadData()
        {
            var categories = new[]
            {
                new
                {
                    Id = 1,
                    Name = "Điện thoại"
                },

                new
                {
                    Id = 2,
                    Name = "Laptop"
                },

                new
                {
                    Id = 3,
                    Name = "Phụ kiện"
                }
            };

            cboCategory.DataSource =
                categories;

            cboCategory.DisplayMember =
                "Name";

            cboCategory.ValueMember =
                "Id";

            bindingSource.DataSource =
                products;

            dgvProducts.DataSource =
                bindingSource;

            UpdateStatus();
        }

    
        private void RegisterEvents()
        {
            dgvProducts.SelectionChanged +=
                Grid_SelectionChanged;

            txtSearch.TextChanged +=
                Search_TextChanged;
        }


        private void Add_Click(
            object sender,
            EventArgs e)
        {
            if (!ValidateInput())
                return;

            Product product =
                new Product();

            product.ProductId =
                txtProductId.Text.Trim();

            product.ProductName =
                txtProductName.Text.Trim();

            product.CategoryId =
                Convert.ToInt32(
                    cboCategory.SelectedValue);

            product.Category =
                cboCategory.Text;

            product.UnitPrice =
                decimal.Parse(
                    txtUnitPrice.Text);

            product.Quantity =
                int.Parse(
                    txtQuantity.Text);

            if (picAvatar.Tag != null)
            {
                product.ImagePath =
                    picAvatar.Tag.ToString();
            }

            products.Add(product);

            bindingSource.ResetBindings(false);

            UpdateStatus();

            MessageBox.Show(
                "Thêm sản phẩm thành công!",
                "Thông báo",
                MessageBoxButtons.OK,
                MessageBoxIcon.Information);

            ClearInput();
        }


        private void Update_Click(
            object sender,
            EventArgs e)
        {
            if (dgvProducts.CurrentRow == null)
            {
                MessageBox.Show(
                    "Hãy chọn sản phẩm cần cập nhật!");

                return;
            }

            if (!ValidateInput())
                return;

            Product product =
                dgvProducts.CurrentRow
                .DataBoundItem as Product;

            if (product == null)
                return;

            product.ProductId =
                txtProductId.Text.Trim();

            product.ProductName =
                txtProductName.Text.Trim();

            product.CategoryId =
                Convert.ToInt32(
                    cboCategory.SelectedValue);

            product.Category =
                cboCategory.Text;

            product.UnitPrice =
                decimal.Parse(
                    txtUnitPrice.Text);

            product.Quantity =
                int.Parse(
                    txtQuantity.Text);

            if (picAvatar.Tag != null)
            {
                product.ImagePath =
                    picAvatar.Tag.ToString();
            }

            bindingSource.ResetBindings(false);

            MessageBox.Show(
                "Cập nhật thành công!",
                "Thông báo",
                MessageBoxButtons.OK,
                MessageBoxIcon.Information);
        }


        private void Delete_Click(
            object sender,
            EventArgs e)
        {
            if (dgvProducts.CurrentRow == null)
            {
                MessageBox.Show(
                    "Hãy chọn sản phẩm cần xóa!");

                return;
            }

            DialogResult result =
                MessageBox.Show(
                    "Bạn có chắc chắn muốn xóa sản phẩm này?",
                    "Xác nhận xóa",
                    MessageBoxButtons.YesNo,
                    MessageBoxIcon.Question);

            if (result != DialogResult.Yes)
                return;

            Product product =
                dgvProducts.CurrentRow
                .DataBoundItem as Product;

            if (product != null)
            {
                products.Remove(product);
            }

            UpdateStatus();

            ClearInput();
        }

 
        private bool ValidateInput()
        {
            bool valid = true;

            errorProvider.Clear();

            if (string.IsNullOrWhiteSpace(
                txtProductName.Text))
            {
                errorProvider.SetError(
                    txtProductName,
                    "Tên SP không được để trống.");

                valid = false;
            }

            decimal price;

            if (!decimal.TryParse(
                txtUnitPrice.Text,
                out price)
                || price <= 0)
            {
                errorProvider.SetError(
                    txtUnitPrice,
                    "Đơn giá phải lớn hơn 0.");

                valid = false;
            }

            int quantity;

            if (!int.TryParse(
                txtQuantity.Text,
                out quantity)
                || quantity < 0)
            {
                errorProvider.SetError(
                    txtQuantity,
                    "Số lượng phải >= 0.");

                valid = false;
            }

            return valid;
        }


        private void ChooseImage_Click(
            object sender,
            EventArgs e)
        {
            using OpenFileDialog dialog =
                new OpenFileDialog();

            dialog.Title =
                "Chọn ảnh sản phẩm";

            dialog.Filter =
                "Image Files|*.png;*.jpg;*.jpeg;*.bmp";

            if (dialog.ShowDialog() ==
                DialogResult.OK)
            {
                try
                {
                    if (picAvatar.Image != null)
                    {
                        picAvatar.Image.Dispose();
                    }

                    picAvatar.Image =
                        Image.FromFile(
                            dialog.FileName);

                    picAvatar.Tag =
                        dialog.FileName;
                }
                catch
                {
                    MessageBox.Show(
                        "Không thể mở ảnh!");
                }
            }
        }

 
        private void Grid_SelectionChanged(
            object sender,
            EventArgs e)
        {
            if (dgvProducts.CurrentRow == null)
                return;

            Product product =
                dgvProducts.CurrentRow
                .DataBoundItem as Product;

            if (product == null)
                return;

            txtProductId.Text =
                product.ProductId;

            txtProductName.Text =
                product.ProductName;

            txtUnitPrice.Text =
                product.UnitPrice.ToString();

            txtQuantity.Text =
                product.Quantity.ToString();

            cboCategory.SelectedValue =
                product.CategoryId;

            if (!string.IsNullOrEmpty(
                product.ImagePath)
                && File.Exists(
                    product.ImagePath))
            {
                try
                {
                    if (picAvatar.Image != null)
                    {
                        picAvatar.Image.Dispose();
                    }

                    picAvatar.Image =
                        Image.FromFile(
                            product.ImagePath);

                    picAvatar.Tag =
                        product.ImagePath;
                }
                catch
                {
                    picAvatar.Image = null;
                }
            }
            else
            {
                picAvatar.Image = null;
                picAvatar.Tag = null;
            }
        }

        private void Search_TextChanged(
            object sender,
            EventArgs e)
        {
            string keyword =
                txtSearch.Text.Trim();

            if (keyword == "")
            {
                bindingSource.DataSource =
                    products;

                return;
            }

            var result =
                products
                .Where(p =>
                    p.ProductName
                    .IndexOf(
                        keyword,
                        StringComparison.OrdinalIgnoreCase)
                    >= 0)
                .ToList();

            bindingSource.DataSource =
                new BindingList<Product>(
                    result);
        }


        private void ExportCsv_Click(
            object sender,
            EventArgs e)
        {
            using SaveFileDialog dialog =
                new SaveFileDialog();

            dialog.Title =
                "Xuất danh sách sản phẩm";

            dialog.Filter =
                "CSV files (*.csv)|*.csv";

            dialog.FileName =
                "products.csv";

            if (dialog.ShowDialog() !=
                DialogResult.OK)
            {
                return;
            }

            StringBuilder csv =
                new StringBuilder();

            csv.AppendLine(
                "Mã SP,Tên SP,Danh Mục,Đơn Giá,Số Lượng");

            foreach (Product product in products)
            {
                csv.AppendLine(
                    $"{EscapeCsv(product.ProductId)}," +
                    $"{EscapeCsv(product.ProductName)}," +
                    $"{EscapeCsv(product.Category)}," +
                    $"{product.UnitPrice}," +
                    $"{product.Quantity}");
            }

            File.WriteAllText(
                dialog.FileName,
                csv.ToString(),
                Encoding.UTF8);

            MessageBox.Show(
                "Xuất CSV thành công!",
                "Thông báo",
                MessageBoxButtons.OK,
                MessageBoxIcon.Information);
        }

        private string EscapeCsv(
            string value)
        {
            if (value.Contains(",") ||
                value.Contains("\""))
            {
                return "\"" +
                       value.Replace(
                           "\"",
                           "\"\"") +
                       "\"";
            }

            return value;
        }

      
        private void Exit_Click(
            object sender,
            EventArgs e)
        {
            Application.Exit();
        }


        private void UpdateStatus()
        {
            statusLabel.Text =
                $"Tổng số sản phẩm: {products.Count}";
        }


        private void ClearInput()
        {
            txtProductId.Clear();
            txtProductName.Clear();
            txtUnitPrice.Clear();
            txtQuantity.Clear();

            if (cboCategory.Items.Count > 0)
            {
                cboCategory.SelectedIndex = 0;
            }

            if (picAvatar.Image != null)
            {
                picAvatar.Image.Dispose();
                picAvatar.Image = null;
            }

            picAvatar.Tag = null;

            errorProvider.Clear();

            dgvProducts.ClearSelection();
        }


        private Label CreateLabel(
            string text)
        {
            Label label =
                new Label();

            label.Text = text;
            label.AutoSize = true;
            label.Anchor =
                AnchorStyles.Left;

            return label;
        }

        private void AddTextBox(
            TableLayoutPanel table,
            string labelText,
            out TextBox textBox)
        {
            Label label =
                CreateLabel(labelText);

            textBox =
                new TextBox();

            textBox.Dock =
                DockStyle.Fill;

            AddRow(
                table,
                label,
                textBox);
        }

        private void AddRow(
            TableLayoutPanel table,
            Control label,
            Control control)
        {
            int row =
                table.RowCount;

            table.RowCount++;

            table.RowStyles.Add(
                new RowStyle(
                    SizeType.AutoSize));

            table.Controls.Add(
                label,
                0,
                row);

            table.Controls.Add(
                control,
                1,
                row);
        }
    }
}
<img width="1200" height="694" alt="1790932019813_5682877835752288061_5682877835752288061_e2e7469a41cfa1362d12fa33ca84185e" src="https://github.com/user-attachments/assets/d0e978eb-3142-4038-a97f-8f3f297b6f2d" />
