Taxonomy Metabox
================

Uma classe para ser utilizada juntamente com a classe Odin_Metabox do Odin Framework, para alterar como os usuários inserem dados de taxonomias no WordPress, criando custom metaboxes.

Muitas vezes os inputs padrão para termos de taxonomias não são adequados às necessidades dos projetos, no caso de taxonomias como tags, as vezes é desejável ter uma listagem das tags já cadastradas para a sua escolha durante a edição de uma publicação. Pensando nessa aplicação e nos possíveis desdobramentos e extensões que possam surgir sobre a modificação dos inputs de taxonomias, essa classe foi criada para auxiliar aqueles que se deparam com essa necessidade em seus projetos, seja servindo com sua utilização, ou mesmo como referência para a criação de sua própria classe.

Odin Framework
--------------

O Odin Framework possui uma classe de criação de custom metaboxes bem estruturada e organizada. Têm se mostrado um tema base bem aceito pela comunidade e possuí uma boa documentação para criação de metaboxes. Além disso, sua classe pode ser implementada em outros temas com facilidade, basta copiar alguns arquivos e modificar alguns diretórios.

Utilizando a classe no Odin Framework
-------------------------------------
Para utilizar a classe com o Odin Framework, basta colocar o arquivo class-metabox-taxonomy.php no diretório '../odin/core/classes'.

No seu functions.php, basta inserir:
```php
require_once get_template_directory() . '/core/classes/class-metabox.php';
require_once get_template_directory() . '/core/classes/class-metabox-taxonomy.php';
```
Pronto! Agora você estará pronto para utilizar a classe no seu functions.php.

Utilizando a classe em outros temas
-----------------------------------

Para utilizar a classe em outros temas, copie a pasta assets, os arquivos class-metabox.php e class-metabox-taxonomy.php em um diretório do seu tema.

No arquivo class-metabox.php, altere o caminho do diretório '/core/assets/...' para o caminho corrreto do diretório em seu tema. Se você tiver colocado os arquivos em um diretório chamado core na raiz do tema, não será preciso alterar nada.

No seu functions.php, basta inserir:
```php
require_once get_template_directory() . '/[meu_caminho]/class-metabox.php';
require_once get_template_directory() . '/[meu_caminho]/class-metabox-taxonomy.php';
```

## Criando campos ##

A criação de campos segue a base de sua classe pai Odin_Metabox, a qual você pode ver a documentação em https://github.com/wpbrasil/odin/wiki/Classe-Odin_Metabox

Atualmente a classe oferece a possibilidade de criação de dois campos:
- tags_checkbox
- tags_select

### Tags Checkbox ###

```php
  $color = new Taxonomy_Metabox(
	    'cores',  // Slug/ID do Metabox (obrigatório)
	    'Cores',  // Nome do Metabox  (obrigatório)
	    'post',   // Slug do Post Type (opcional)
	    'normal', // Contexto (opções: normal, advanced, ou side) (opcional)
	    'high',   // Prioridade (opções: high, core, default ou low) (opcional)
	    'cor'     // Slug da taxonomia
	);
	
	/* É preciso realizar uma busca por todos os termos da taxonomia
	 * para passá-los no array de options do campo.
	 */
	$colors = get_terms('cor', 'hide_empty=0'); 
	
	/* Caso exista termos registrados, iterar sobre todos os termos e
	 * armazená-los em um array, associando term->name com term->slug.
	 */
	if( $colors ) {
			foreach ($colors as $cor) {
			      $boxes[$cor->name] = $cor->slug;
		    }
		
		/*
		 * Definir os campos passando o type 'tags_checkbox' e o array no options.
		 */
		$color->set_fields( 
		array(
	        array(
	            'id'          => 'cores',
	            'label'       => 'Cores',
	            'type'        => 'tags_checkbox',
	            'attributes'  => array('multiple' => 'multiple'),
	            'description' => __( 'Select the post color', 'odin' ),
	            'options'	    => $boxes
	        )
	    )
		);
	}
```
