---
title: "IonAuth: Simple and Lightweight Auth Library"
description: "Easy way to learn IonAuth for CodeIgniter"
summary: "Easy way to learn IonAuth for CodeIgniter"
date: 2023-11-17T21:48:44+07:00
draft: false
author: "0xprunee" # ["Me", "You"] # multiple authors
tags: ["secure-coding", "php", "library"]
canonicalURL: ""
showToc: true
TocOpen: false
TocSide: 'right'  # or 'left'
# weight: 1
# aliases: ["/first"]
hidemeta: false
comments: false
disableHLJS: true # to disable highlightjs
disableShare: true
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "images/cover.webp" # image path/url
    alt: "Cover: IonAuth: Simple and Lightweight Auth Library" # alt text
    caption: "IonAuth: Simple and Lightweight Auth Library" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
# editPost:
#     URL: ""
#     Text: "Suggest Changes" # edit text
#     appendFilePath: true # to append file path to Edit link
---
## Overview
CodeIgniter is known for its simplicity and flexibility, but setting up a robust authentication system can be a challenge. In this article, we'll explore IonAuth, a tailored authentication library. Discover its features, integration ease, and the added security it brings to CodeIgniter applications.

## Introduction
IonAuth is simple and lightweight authentication on CodeIgniter, its develop by [Ben Edmunds](http://benedmunds.com/). Until this article is published, IonAuth has already released version 4. In this version, IonAuth has made numerous positive changes to the library by adding several features and removing outdated ones to integrate seamlessly with the latest version of CodeIgniter.
IonAuth is a quite popular library, as evidenced by its GitHub stats: they have garnered 💜 1.2k forks and ⭐ 2.3k stars. These numbers underscore IonAuth's widespread adoption, with a substantial user base and active contributions.

![Github IonAuth](./images/github-ionauth.webp#center)

### License
Ion Auth is released under the Apache License v2.0. You can read the license here: http://www.apache.org/licenses/LICENSE-2.0

### Feature
Some of the features introduced by IonAuth 4 include:
- Bringing several hashing methods, such as bcrypt (default), and argon2.
- Offering a variety of template options, including templates for login, register, forgot password, email activation, etc.
- Easy-to-understand and simple configuration.
For more details, you can read further on https://github.com/benedmunds/CodeIgniter-Ion-Auth/blob/4/USERGUIDE.md.

## Steps
<aside>💡 System requirements: before install Ion Auth 4, we needs **CodeIgniter 4** and **PHP**≥**7.2**.</aside>

### Manual Installation
1. Download the latest version: https://github.com/benedmunds/CodeIgniter-Ion-Auth/zipball/4
2. Copy the files from this package to the correspoding folder in your application folder. For example, copy IonAuth/Config/IonAuth.php to app/Config/IonAuth.php.
3. You can also copy the entire directory structure into your ThirdParty/ folder. For example, copy everything to app/ThirdParty/IonAuth/
4. Use the migration file (in Database/Migrations/)
    
    ```
    $ php spark migrate:latest -n IonAuth
    ```
5. Insert default data (Don't forget to set Config\Migrations:enabled to true.) 
**Windows :**
```
$ php spark db:seed IonAuth\Database\Seeds\IonAuthSeeder
```
**Linux :**
```jsx
$ php spark db:seed IonAuth\\Database\\Seeds\\IonAuthSeeder
```

### Composer Installation
For an existing composer project:
```
$ composer config minimum-stability dev
$ composer config repositories.ionAuth vcs git@github.com:benedmunds/CodeIgniter-Ion-Auth.git
$ composer require benedmunds/codeigniter-ion-auth:4.x-dev
```
For a new project:
```jsx
$ composer init
$ composer config minimum-stability dev
$ composer config repositories.ionAuth vcs git@github.com:benedmunds/CodeIgniter-Ion-Auth.git
$ composer require benedmunds/codeigniter-ion-auth:4.x-dev
```

## How to use
The simplest way to use it is by calling the IonAuth library into the controller, for example:
```php
/**
 * Class Auth
 * @property Ion_auth|Ion_auth_model $ion_auth        The ION Auth spark
 * @property CI_Form_validation      $form_validation The form validation library
 */
class Auth extends CI_Controller
{
	public $data = [];

	public function __construct()
	{
		parent::__construct();
		$this->load->database();
		$this->load->library(['ion_auth', 'form_validation']);
```

After that, you are free to use it, for example, to create a simple login with authentication from IonAuth to check if the data matches what is in the database, as follows:

```php
public function login()
	{
		$this->data['title'] = $this->lang->line('login_heading');

		// validate form input
		$this->form_validation->set_rules('identity', str_replace(':', '', $this->lang->line('login_identity_label')), 'required');
		$this->form_validation->set_rules('password', str_replace(':', '', $this->lang->line('login_password_label')), 'required');

		if ($this->form_validation->run() === TRUE)
		{
			// check to see if the user is logging in
			// check for "remember me"
			$remember = (bool)$this->input->post('remember');

			// verify data in DB with ionauth
			if ($this->ion_auth->login($this->input->post('identity'), $this->input->post('password'), $remember))
			{
				//if the login is successful
				//redirect them back to the home page
				$this->session->set_flashdata('message', $this->ion_auth->messages());
				redirect('/', 'refresh');
			}
			else
			{
				// if the login was un-successful
				// redirect them back to the login page
				$this->session->set_flashdata('message', $this->ion_auth->errors());
				redirect('auth/login', 'refresh'); // use redirects instead of loading views for compatibility with MY_Controller libraries
			}
		}
		else
		{
			// the user is not logging in so display the login page
			// set the flash data error message if there is one
			$this->data['message'] = (validation_errors()) ? validation_errors() : $this->session->flashdata('message');

			$this->data['identity'] = [
				'name' => 'identity',
				'id' => 'identity',
				'type' => 'text',
				'value' => $this->form_validation->set_value('identity'),
			];

			$this->data['password'] = [
				'name' => 'password',
				'id' => 'password',
				'type' => 'password',
			];

			$this->_render_page('auth' . DIRECTORY_SEPARATOR . 'login', $this->data);
		}
	}
```
You can explore IonAuth further and customize it according to your preferences. Learn more about it at https://github.com/benedmunds/CodeIgniter-Ion-Auth/blob/4/USERGUIDE.md.

## Support
The IonAuth developer, benemuds, welcomes contributions from those who find bugs in the library. You can visit [IonAuth issues](https://github.com/benedmunds/CodeIgniter-Ion-Auth/issues) to report issues.
If you need customization or assistance with implementing IonAuth in your company, you can directly contact [their email](mailto:ionauth_support_contract@benedmunds.com).
Don't forget to give a ⭐ on the official IonAuth GitHub repository as a token of appreciation and support for the work.

## Conclusion
IonAuth is an alternative library that you can use when building web applications with the CodeIgniter framework. Apart from providing speed, it is also very easy to understand. Feel free to explore it in more detail on your own.

See you in the next article! 

## References
- https://github.com/benedmunds/CodeIgniter-Ion-Auth
- http://benedmunds.com/ion_auth/