//Una biblioteca necesita un sistema para procesar la información de los libros. De cada libro se conoce: ISBN, código del
//autor y código de género (1 a 15).
//a) Implementar un módulo que lea información de los libros y retorne una estructura de datos eficiente para la
//búsqueda por código de autor que contenga código de autor y una lista de todos sus libros. La lectura finaliza al
//ingresar el valor 0 para un ISBN.
//b) Realizar un módulo que reciba la estructura generada en el inciso a), un código de autor y un código de género.
//El módulo debe retornar una lista con código de autor y cantidad de libros del código de género recibido, para
//cada autor cuyo código sea superior al código de autor ingresado.
//c) Realizar un módulo recursivo que reciba la estructura generada en inciso b) y retorne cantidad y código de autor
//con mayor cantidad de libros.
//NOTA: Implementar el programa principal, que invoque a los incisos a, b y c. En caso de ser necesario, puede utilizar los
//módulos que se encuentran a continuación.

program biblioteca;
Const
	max_genero=15;
	isbn_fin=0;
Type
	rango_gen= 1..max_genero;
	
	libro=record
		isbn:integer;
		codautor:integer;
		codgen:rango_gen;
	end;
	
	regLista=record
		isbn:integer;
		codgen:rango_gen;
	end;
	
	listaLibros=^nodol;
	nodol=record
		dato:regLista;
		sig:listaLibros;
	end;
	
	regArbol=record
		codautor:integer;
		lista:listaLibros;
	end;
	
	arbol=^nodoArbol;
	nodoArbol= record
		dato:regArbol;
		hi:arbol;
		hd:arbol;
	end;
	
	regListaNueva= record
		codautor:integer;
		cantl:integer;
	end;
	
	listaNueva=^nodoln;
	nodoln= record
		dato:regListaNueva;
		sig:listaNueva;
	end;
		
	
//-------------CARGAR ARBOL-------------------
procedure cargarArbol(var a:arbol);

procedure leerLibro(var l:libro);
begin
	l.isbn:= random(50);
	if(l.isbn <> isbn_fin)then begin
		l.codautor:= random(100);
		l.codgen:= 1+ random(15);
	end;
end;
procedure cargarRegistro(l:libro; var rl:regLista);
begin
	rl.isbn:=l.isbn;
	rl.codgen:=l.codgen;
end;
procedure agregarAdelante(var l:listaLibros; rl:regLista);
var
	nue:listaLibros;
begin
	new(nue);
	nue^.dato:=rl;
	nue^.sig:=l;
	l:=nue;
end;

procedure insertarArbol(var a:arbol; codAutor:integer; rl:regLista);
begin
	if(a=nil)then begin
		new(a);
		a^.dato.lista:=NIL;
		a^.dato.codautor:=codAutor;
		a^.HD:=NIL;
		a^.HI:=NIL;
		agregarAdelante(a^.dato.lista,rl);
	end
	else if(a^.dato.codautor= codAutor)then 
		agregarAdelante(a^.dato.lista,rl)
		else
			if(a^.dato.codautor>codAutor)then
				insertarArbol(a^.HI, codAutor,rl)
			else
				insertarArbol(a^.HD, codAutor,rl);
end;

var
	l:libro;
	rl:regLista;
begin
	leerLibro(l);
	while(l.isbn <>isbn_fin) do begin
		cargarRegistro(l,rl);
		insertarArbol(a,l.codautor,rl);
		leerLibro(l);
	end;
end;
///---------------------------------------------
procedure imprimirLista(l:listaLibros);
begin
	while(l<>nil)do begin
		writeln('isbn:   ',l^.dato.isbn);
		writeln('codigo de genero:  ',l^.dato.codgen);
		l:=l^.sig;
	end;
end;
procedure imprimirArbol(a:arbol);
begin
	if(a<>nil)then begin
		imprimirArbol(a^.HI);
		writeln('-------------------');
		writeln('Codigo de autor:       ',a^.dato.codautor);
		writeln('------LISTA:  ');
		imprimirLista(a^.dato.lista);
		imprimirArbol(a^.HD);
	end;
end;
//b) Realizar un módulo que reciba la estructura generada en el inciso a), un código de autor y un código de género.
//El módulo debe retornar una lista con código de autor y cantidad de libros del código de género recibido, para
//cada autor cuyo código sea superior al código de autor ingresado.
procedure agregarAdelante(var ln:listaNueva; rln:regListaNueva);
var
	nue:listaNueva;
begin
	new(nue);
	nue^.dato:= rln;
	nue^.sig:=ln;
	ln:=nue;
end;
procedure contarLibros(l:listaLibros; codautor:integer;codgen:integer;var ln:listaNueva);
var
	rln:regListaNueva;
begin
	rln.codautor:=codautor;
	rln.cantl:=0;
	while(l<>nil)do begin
		if(l^.dato.codgen=codgen)then 
			rln.cantl:= rln.cantl+1;
		l:=l^.sig;
	end;
	agregarAdelante(ln,rln);
end;


procedure generarListaNueva(a:arbol; var ln:listaNueva; codautor:integer; codgen:integer);
begin
	if(a<>nil)then begin
		if(a^.dato.codautor > codautor)then begin
			contarLibros(a^.dato.lista,a^.dato.codautor, codgen, ln);
			generarListaNueva(a^.HI, ln,codautor, codgen);
			generarListaNueva(a^.HD,ln,codautor,codgen);
		end;
	end;
end;

procedure imprimirListaNueva(l:listaNueva);
begin
	while (l<>nil)do begin
		writeln('---------------');
		writeln('El codigo de autor:  ', l^.dato.codautor);
		writeln('La cantidad de libros:   ', l^.dato.cantl);
		writeln('--------------');
		l:=l^.sig;
	end;
end;
//c) Realizar un módulo recursivo que reciba la estructura generada en inciso b) y retorne cantidad y código de autor
//con mayor cantidad de libros.

procedure obtenerMayor(ln:listaNueva; var maxcant,codmax:integer);
begin
	if(ln<>nil)then begin
		if(ln^.dato.cantl> maxcant)then begin
			maxcant:=ln^.dato.cantl;
			codmax:= ln^.dato.codautor;
		end;
		obtenerMayor(ln^.sig, maxcant,codmax);
	end;
end;

//---------------PROGRAMA PRINCIPAL-----------------
var
	a:arbol; 
	codautor,codgen:integer;
	ln:listaNueva;
	maxcant,codmax:integer;
begin
	a:=nil;
	randomize;
	cargarArbol(a);
	writeln('--------ARBOL--------');
	imprimirArbol(a);
	writeln('Ingrese un codigo de autor'); readln(codautor);
	writeln('Ingrese un codigo de genero'); readln(codgen);
	generarListaNueva(a,ln,codautor,codgen);
	imprimirListaNueva(ln);
	maxcant:=-999;
	codmax:=-1;
	obtenerMayor(ln,maxcant,codmax);
	writeln('El codigo del autor con mayor cantidad de ventas:    ' ,codmax, 'con:    ', maxcant , ' ventas '); 
end.
