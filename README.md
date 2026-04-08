# praktikum1-4

# PRAKTIKUM 1

# 1. Installasi Codeigniter
<img width="1820" height="980" alt="Screenshot (229)" src="https://github.com/user-attachments/assets/11114c21-3c5d-4636-9721-3a7cef8245c7" />

# 2. Mengaktifkan mode Debugging

<img width="1289" height="750" alt="Cuplikan layar 2026-04-07 191557" src="https://github.com/user-attachments/assets/8f067770-1255-4f10-81ad-6f625ba7e164" />

# 3. Membuat Controller

<img width="1820" height="980" alt="Screenshot (241)" src="https://github.com/user-attachments/assets/46d525aa-415f-4fb7-9a56-6d68219b42a6" />

# 4. Membuat Layout css

<img width="1820" height="980" alt="Screenshot (244)" src="https://github.com/user-attachments/assets/72e7186a-4d71-4a94-9fdd-209dc37311e9" />

# PRAKTIKUM 2

# 1. Membuat Database dan Tabel

<img width="1820" height="980" alt="image" src="https://github.com/user-attachments/assets/57a168e7-de5b-4a73-ab70-8ec5018a5671" />

# 2. Membuat model

<img width="1820" height="980" alt="Screenshot (244)" src="https://github.com/user-attachments/assets/443d8f31-075e-4331-a925-0ff14c2ef7de" />

# 3. Membuat Tampilan Detail Artikel

<img width="1820" height="980" alt="Screenshot (252)" src="https://github.com/user-attachments/assets/0796a2b6-e761-4cce-8e0c-a9c9d75c34d5" />
<img width="1820" height="980" alt="Screenshot (254)" src="https://github.com/user-attachments/assets/647ce68f-cb3a-409e-88f2-ddde8107c6d0" />


# 4. Membuat Menu Admin CRUD

<img width="1820" height="980" alt="Screenshot (255)" src="https://github.com/user-attachments/assets/023ad4ae-7ffc-4378-a208-177b98356429" />


# PRAKTIKUM 3



# 1. Model artikel pengendali

```
public function index()
{
    $title = 'Daftar Artikel';
    $model = new ArtikelModel();
    $artikel = $model->findAll();

    return view('artikel/index', compact('artikel', 'title'));
}
```

# 2. Detail artikel Berdasarkan Slug

```
public function view($slug)
{
    $model = new ArtikelModel();
    $artikel = $model->where(['slug' => $slug])->first();

    if (! $artikel) {
        throw PageNotFoundException::forPageNotFound();
    }

    $title = $artikel['judul'];
    return view('artikel/detail', compact('artikel', 'title'));
}
```

# PRAKTIKUM 4

- Membuat formulir login
- Mengambil masukan email dan kata sandi
- Konversi login dengan tabel user
- Menggunakanpassword_verify()
- Simpan login sesi
- Membuat logout
- Membatasi akses admin menggunakan filter auth

# 1. Membuat Database

<img width="1404" height="745" alt="image" src="https://github.com/user-attachments/assets/a8c833cb-5671-4bc3-8f6d-d777489c55e4" />

# 2. Membuat user.php dan login.php

# user php

```
<?php
namespace App\Controllers;
use App\Models\UserModel;

class User extends BaseController
{
    public function index()
    {
        $title = 'Daftar User';
        $model = new UserModel();
        $users = $model->findAll();
        return view('user/index', compact('users', $title));
    }

    public function login()
    {
        helper(['form']);
        $email = $this->request->getPost('email');
        $password = $this->request->getPost('password');

        if (!$email) {
            return view('user/login');
        }

        $session = session();
        $model = new UserModel();
        $login = $model->where('useremail', $email)->first();

        if ($login) {
            $pass = $login['userpassword'];
            if (password_verify($password, $pass)) {
                $login_data = [
                    'user_id' => $login['id'],
                    'user_name' => $login['username'],
                    'user_email' => $login['useremail'],
                    'logged_in' => TRUE,
                ];
                $session->set($login_data);
                return redirect('admin/artikel');
            } else {
                $session->setFlashdata("flash_msg", "Password salah.");
                return redirect()->to('/user/login');
            }
        } else {
            $session->setFlashdata("flash_msg", "email tidak terdaftar.");
            return redirect()->to('/user/login');
        }
    }

    public function logout()
    {
        session()->destroy();
        return redirect()->to('/user/login');
    }
}
```

<img width="1820" height="980" alt="image" src="https://github.com/user-attachments/assets/75719d4f-2695-427d-aa43-80016d6abb91" />

