Formularios
Los elementos de formularios en HTML funcionan un poco diferente
 a otros elementos del DOM en React, debido a que los elementos de 
 formularios conservan naturalmente algún estado interno.
  Por ejemplo, este formulario solamente en HTML, 
  acepta un solo nombre.
  <form>
    <label>
      Name:
      <input type="text" name="name" />
    </label>
    <input type="submit" value="Submit" />
  </form>

  Este formulario tiene el comportamiento predeterminado en HTML 
  que consiste en navegar a una nueva página cuando el usuario envía
   el formulario. Si deseas este comportamiento en React, simplemente
   a funciona así. Pero en la mayoría de casos, es conveniente tener
    una función en Javascript que se encargue del envío del formulario,
     y que tenga acceso a los datos que el usuario introdujo en el 
     formulario. La forma predeterminada para conseguir esto es una
      técnica llamada “componentes controlados”

      Componentes controlados
    En HTML, los elementos de formularios como los
    <input>, <textarea> y el <select> normalmente mantienen sus
    propios estados y los actualizan de acuerdo a la
     interacción del usuario. En React, el estado mutable es 
     mantenido normalmente en la propiedad estado de los componentes,
      y solo se actualiza con setState().
Podemos combinar ambos haciendo que el estado de React sea la “única
 fuente de la verdad”. De esta manera, los componentes React que 
rendericen un formulario también controlan lo que pasa en ese 
formulario con las subsecuentes entradas del usuario. Un campo de un
 formulario cuyos valores son controlados por React de esta forma es 
denominado “componente controlado”.

class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}

Ya que el atributo value es agregado en nuestro elemento del formulario, el valor mostrado siempre será el de this.state.value, haciendo que el estado de React sea la fuente de la verdad. Ya que handleChange corre cada vez que una tecla es oprimida para actualizar el estado de React, el valor mostrado será actualizado mientras que el usuario escribe.

La etiqueta textarea
En HTML, el elemento <textarea> define su texto por sus hijos:
<textarea>
  Hello there, this is some text in a text area
</textarea>

En React, un <textarea> utiliza un atributo value en su lugar. De esta manera, un formulario que hace uso de un <textarea> puede ser escrito de manera similar a un formulario que utiliza un campo en una sola línea:

class EssayForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 'Please write an essay about your favorite DOM element.'
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('An essay was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Essay:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
Recuerda que this.state.value es inicializado en el constructor, de manera que el área de texto empiece con algo de texto.

La etiqueta select
En HTML, <select> crea una lista desplegable. Por ejemplo, este HTML crea una lista desplegable de sabores:

<select>
  <option value="grapefruit">Grapefruit</option>
  <option value="lime">Lime</option>
  <option selected value="coconut">Coconut</option>
  <option value="mango">Mango</option>
</select>
Ten en cuenta que la opción Coco es inicialmente seleccionada, debido al atributo selected. React, en lugar de utilizar el atributo selected, utiliza un atributo value en la raíz de la etiqueta select. Esto es más conveniente en un componente controlado debido a que solo necesitas actualizarlo en un solo lugar, por ejemplo:

class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: 'coconut'};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('Your favorite flavor is: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite flavor:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">Grapefruit</option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}

Puedes pasar un array al atributo value, permitiendo que selecciones múltiples opciones en una etiqueta select:

<select multiple={true} value={['B', 'C']}>

La etiqueta file input
En HTML, un <input type="file"> permite que el usuario escoja uno o varios archivos de su dispositivo de almacenamiento para ser cargados a un servidor o ser manipulados por Javascript mediante el API de Archivos.

<input type="file" />