unit codigo_agenda;

{$mode objfpc}{$H+}

interface

uses
  Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls;


const
 C_FNAME = 'Agenda.DAT';

type
  vectorStrings = array of string;

  contacto = record
       nombre:STRING[40];
       direccion:STRING[150];
       telefono:STRING[50];
       correo:STRING[50];
end;
tAgenda = array[1..25] of contacto;
  { TfrmAgenda }

  TfrmAgenda = class(TForm)
    btnPrimero: TButton;
    btnAnterior: TButton;
    btnSiguiente: TButton;
    btnUltimo: TButton;
    btnNuevo: TButton;
    btnGuardar: TButton;
    btnCancelar: TButton;
    btnEditar: TButton;
    btnEliminar: TButton;
    Label6: TLabel;
    Label7: TLabel;
    lblNumero: TLabel;
    txtCorreo: TEdit;
    txtTelefono: TEdit;
    txtDireccion: TEdit;
    txtNombre: TEdit;
    Label1: TLabel;
    Label2: TLabel;
    Label3: TLabel;
    Label4: TLabel;
    Label5: TLabel;
    procedure btnAnteriorClick(Sender: TObject);
    procedure btnCancelarClick(Sender: TObject);
    procedure btnEditarClick(Sender: TObject);
    procedure btnEliminarClick(Sender: TObject);
    procedure btnGuardarClick(Sender: TObject);
    procedure btnNuevoClick(Sender: TObject);
    procedure btnPrimeroClick(Sender: TObject);
    procedure btnSiguienteClick(Sender: TObject);
    procedure btnUltimoClick(Sender: TObject);
    procedure txtDireccionChange(Sender: TObject);
    procedure FormCreate(Sender: TObject);
  private
    { private declarations }
  public
    { public declarations }
  end;

var
  frmAgenda: TfrmAgenda;
   i:integer;
   n:integer;
   vAgenda:tAgenda;
   bandEditar:Boolean;

implementation

{$R *.lfm}

function Split(Texto, Delimitador: string): vectorStrings;

var
  o: integer;
  PosDel: integer;
  Aux: string;

begin

  o := 0;
  Aux := Texto;
  SetLength(Result, Length(Aux));

  repeat

    PosDel := Pos(Delimitador, Aux) - 1;

    if PosDel = -1 then
    begin
      Result[o] := Aux;
      break;
    end;

    Result[o] := copy(Aux, 1, PosDel);
    delete(Aux, 1, PosDel + Length(Delimitador));
    inc(o);
  until Aux = '';
end;
procedure cargarDeVectoraArchivo(Sender: TObject);
var
 tfOut: TextFile;
 j: integer;


begin
       // Set the name of the file that will be read
  AssignFile(tfOut, C_FNAME);

    // Use exceptions to catch errors (this is the default so not absolutely requried)
  {$I+}

  // Embed the file creation in a try/except block to handle errors gracefully
  try
    // Create the file, write some text and close it.
    rewrite(tfOut);

    for j := 1 to n do
      begin
        with vAgenda[j] do
           begin
                if (nombre <>'') then
                    begin
                    writeln(tfOut, nombre,'|',direccion,'|', telefono,'|',correo);
                    end;
           end;

      end;

    CloseFile(tfOut);

  except
    // If there was an error the reason can be found here
    on E: EInOutError do
      writeln('File handling error occurred. Details: ', E.ClassName, '/', E.Message);
  end;


end;
procedure cargarDeArchivoaVector(Sender: TObject);
var
 tfIn: TextFile;
 s: string;
 vContactos:vectorStrings ;

begin
       // Set the name of the file that will be read
  AssignFile(tfIn, C_FNAME);

  // Embed the file handling in a try/except block to handle errors gracefully
  try
    // Open the file for reading
    reset(tfIn);
    n:=1;
    i:=1;
    // Keep reading lines until the end of the file is reached
    while not eof(tfIn) do
    begin
      readln(tfIn, s);
      vContactos := Split(s,'|');

      //Cargar el vector de contactos
      with vAgenda[n] do
           begin
                    nombre:= vContactos[0];
                    direccion:= vContactos[1];
                    telefono:= vContactos[2];
                    correo:= vContactos[3];
           end;
      if (n=1) then
      begin
            //      ShowMessage(vContactos[1]);
              frmAgenda.lblNumero.caption := '1';
              frmAgenda.txtNombre.text := vContactos[0];
              frmAgenda.txtDireccion.text := vContactos[1];
              frmAgenda.txtTelefono.text := vContactos[2];
              frmAgenda.txtCorreo.text := vContactos[3];

      end;
      n:=n+1;
    end;
       n:=n-1;
    // Done so close the file
    CloseFile(tfIn);

  except
    on E: EInOutError do
     writeln('File handling error occurred. Details: ', E.Message);
  end;

end;

procedure mostrarRegistro(Sender: TObject);
begin
  frmAgenda.lblNumero.caption:= InttoStr(i);
  frmAgenda.txtNombre.text := vAgenda[i].nombre;
  frmAgenda.txtDireccion.text := vAgenda[i].direccion;
  frmAgenda.txtTelefono.text := vAgenda[i].telefono;
  frmAgenda.txtCorreo.text := vAgenda[i].correo;


end;
procedure cajas(  Lectura: Boolean);
begin
  frmAgenda.txtNombre.ReadOnly := Lectura;
  frmAgenda.txtDireccion.ReadOnly := Lectura;
  frmAgenda.txtCorreo.ReadOnly := Lectura;
  frmAgenda.txtTelefono.ReadOnly := Lectura;
end;

procedure botones(Sender: TObject);
begin

     //Revisar estado de botones
    frmAgenda.btnPrimero.Enabled := True;
    frmAgenda.btnSiguiente.Enabled := True;
    frmAgenda.btnAnterior.Enabled := True;
    frmAgenda.btnUltimo.Enabled := True;
    //ShowMessage(IntToStr(n));

    if ( i=1 ) then
      begin

            frmAgenda.btnAnterior.Enabled:=False;
            frmAgenda.btnPrimero.Enabled := False;
      end;

        if ( i=n ) then
      begin
            frmAgenda.btnSiguiente.Enabled := False;
            frmAgenda.btnUltimo.Enabled := False;
      end;
end;




{ TfrmAgenda }

procedure TfrmAgenda.btnSiguienteClick(Sender: TObject);
begin
    i:=i+1;
    mostrarRegistro(Sender);
    botones(Sender);

end;

procedure TfrmAgenda.btnUltimoClick(Sender: TObject);
begin
  i:=n;
  mostrarRegistro(Sender);
  botones(Sender);
end;

procedure TfrmAgenda.btnAnteriorClick(Sender: TObject);
begin
  i:=i-1;
  mostrarRegistro(Sender);
  botones(Sender);
end;

procedure TfrmAgenda.btnCancelarClick(Sender: TObject);
begin
  btnGuardar.Enabled:=False;
  btnCancelar.Enabled:=False;
  btnNuevo.Enabled:=True;
  btnEliminar.Enabled:=True;
  btnEditar.Enabled:=True;
  bandEditar := False;
  i:=n;
  mostrarRegistro(Sender);
  botones(Sender);
  cajas(False);

end;

procedure TfrmAgenda.btnEditarClick(Sender: TObject);
begin
  btnGuardar.Enabled:=True;
  btnCancelar.Enabled:=True;
  btnNuevo.Enabled:=False;
  btnEliminar.Enabled:=False;
  btnEditar.Enabled:=False;
  cajas(False);
  bandEditar := True;
end;

procedure TfrmAgenda.btnEliminarClick(Sender: TObject);
begin
  case QuestionDlg ('Agenda de contactos','¿Realmente desea eliminar el contacto número: ' + IntToStr(i)+' ?',mtCustom,[mrYes,'Si, deseo eliminarlo', mrNo, 'No', 'IsDefault'],'') of
        mrYes:
          begin
              with vAgenda[i] do
              begin
                    nombre:= '';
                    direccion:= '';
                    telefono:= '';
                    correo:= '';
              end;
            cargarDeVectoraArchivo(Sender);
            cargarDeArchivoaVector(Sender);
            botones(Sender);
            QuestionDlg ('Agenda de contactos','Se ha eliminado el contacto',mtCustom,[mrOK,'Cerrar'],'');

          end;
        mrNo:
          begin;
            QuestionDlg ('Agenda de contactos','Se ha cancelado',mtCustom,[mrOK,'Cerrar'],'');
          end;

    end;
end;

procedure TfrmAgenda.btnGuardarClick(Sender: TObject);
var
    tfOut: TextFile;
begin

   if ( bandEditar = False  ) then
     // Nuevo registro
     begin
       n:=n+1;
       with vAgenda[n] do
               begin
                        nombre:= txtNombre.text;
                        direccion:= txtDireccion.text;
                        telefono:= txtTelefono.text;
                        correo:= txtCorreo.text;
               end;
        i:=n;
        AssignFile(tfOut, C_FNAME);
        try
          append(tfOut);
          writeln(tfOut, txtNombre.Text,'|',txtDireccion.Text,'|', txtTelefono.Text,'|',txtCorreo.Text);
          CloseFile(tfOut);

        except
          on E: EInOutError do
           writeln('File handling error occurred. Details: ', E.Message);
        end;
     end
   else  // Editar registro existente
     begin
         with vAgenda[i] do
               begin
                        nombre:= txtNombre.text;
                        direccion:= txtDireccion.text;
                        telefono:= txtTelefono.text;
                        correo:= txtCorreo.text;
               end;
         cargarDeVectoraArchivo(Sender);

     end;


  botones(Sender);
  btnGuardar.Enabled:=False;
  btnCancelar.Enabled:=False;
  btnNuevo.Enabled:=True;
  btnEliminar.Enabled:=True;
  btnEditar.Enabled:=True;
  cajas(True);
  ShowMessage('Se guardó el contacto correctamente');


end;

procedure TfrmAgenda.btnNuevoClick(Sender: TObject);
begin
  lblNumero.caption:=IntToStr(n+1);
  txtNombre.text:='';
  txtDireccion.text:='';
  txtCorreo.text:='';
  txtTelefono.text:='';
  btnGuardar.Enabled:=True;
  btnCancelar.Enabled:=True;
  btnEliminar.Enabled:=False;
  btnEditar.Enabled:=False;
  btnNuevo.Enabled:=False;
  bandEditar := False;
  cajas(False);
end;

procedure TfrmAgenda.btnPrimeroClick(Sender: TObject);
begin
  i:=1;
  mostrarRegistro(Sender);
  botones(Sender);
end;

procedure TfrmAgenda.FormCreate(Sender: TObject);

begin

  cargarDeArchivoaVector(Sender);
  botones(Sender);

end;


procedure TfrmAgenda.txtDireccionChange(Sender: TObject);
begin

end;

end.

