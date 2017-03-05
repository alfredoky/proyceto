# proyceto
<connectionStrings>
    <clear />
    <add name="examen" connectionString="SERVER=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=LAPTOP-UL0LQ92T)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=XE)));uid=system;pwd=admin;" />
  </connectionStrings>
  
  [WebMethod]
    public string loggin(string u, string p, int perfil)
    {


        connection = new OracleConnection(ConnectionString);
        connection.Open();
        command = new OracleCommand("select RUT,CONTRASENA,TBL_PERFIL_ID from encuesta.TBL_FUNCIONARIO where RUT='" + u + "' and CONTRASENA='" + p + "' and TBL_PERFIL_ID='" + perfil + "'", connection);

        command.CommandType = CommandType.Text;
        OracleDataReader dr = command.ExecuteReader();

        string x;
        x = "";

        if (dr.HasRows && perfil == 1) // file exists in DB
        {

            dr.Read();
            x = "1";
        }
        else if (dr.HasRows && perfil == 2)
        {
            dr.Read();
            x = "2";
        }

        else if (dr.HasRows.Equals(false) || perfil.Equals(false))
        {
            dr.Read();
            x = "3";
        }


        return x;
    }


    [WebMethod]
    public Boolean validaEncuesta()
    {

        connection = new OracleConnection(ConnectionString);
        connection.Open();
        command = new OracleCommand("select RUT_EVALUADO from encuesta.TBL_EVALUACION", connection);
        command.CommandType = CommandType.Text;
        OracleDataReader dr = command.ExecuteReader();

        Boolean x;
        if (dr.HasRows) // file exists in DB
        {
            dr.Read();
            x = true;
        }
        else
        {
            x = false;
        }

        return x;
    }

    [WebMethod]

    public DataSet getData()
    {

        OracleConnection conn = new OracleConnection();
        conn.ConnectionString = "SERVER=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=DESKTOP-E2LDKPF)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=XE)));uid=system;pwd=admin;";
        OracleDataAdapter da = new OracleDataAdapter("select nota_autoevaluacion, nota_jefe, nota_final from encuesta.tbl_notas", conn);
        DataSet ds = new DataSet();
        da.Fill(ds);
        return ds;

    }

    [WebMethod]
    public DataSet getData2()
    {

        OracleConnection conn = new OracleConnection();
        conn.ConnectionString = "SERVER=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=DESKTOP-E2LDKPF)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=XE)));uid=system;pwd=admin;";
        OracleDataAdapter da = new OracleDataAdapter("select * from encuesta.pregunta", conn);
        DataSet ds = new DataSet();
        da.Fill(ds);
        return ds;

    }

    [WebMethod]
    public DataSet getDatajefe()
    {

        OracleConnection conn = new OracleConnection();
        conn.ConnectionString = "SERVER=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=DESKTOP-E2LDKPF)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=XE)));uid=system;pwd=admin;";
        OracleDataAdapter da = new OracleDataAdapter("select * from encuesta.pregunta", conn);
        DataSet ds = new DataSet();
        da.Fill(ds);
        return ds;

    }

    [WebMethod]
    public string GetLocalIPAddress()
    {
        var host = Dns.GetHostEntry(Dns.GetHostName());
        foreach (var ip in host.AddressList)
        {
            if (ip.AddressFamily == AddressFamily.InterNetwork)
            {
                return ip.ToString();
            }
        }
        throw new Exception("Local IP Address Not Found!");
    }

    [WebMethod]

    public string fecha(string fecha)
    {
        fecha = System.DateTime.Now.ToString("yyyy/MM/dd HH:mm:ss");
        return fecha;

    }

    [WebMethod]

    public DataSet cargo()
    {
        OracleConnection conn = new OracleConnection();
        conn.ConnectionString = "SERVER=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=DESKTOP-E2LDKPF)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=XE)));uid=system;pwd=admin;";
        OracleDataAdapter da = new OracleDataAdapter("select nombre from encuesta.tbl_cargo", conn);
        DataSet ds = new DataSet();
        da.Fill(ds);
        return ds;

    }

    [WebMethod]
    public DataSet area()
    {
        OracleConnection conn = new OracleConnection();
        conn.ConnectionString = "SERVER=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=DESKTOP-E2LDKPF)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=XE)));uid=system;pwd=admin;";
        OracleDataAdapter da = new OracleDataAdapter("select nombre from encuesta.tbl_area_investigacion", conn);
        DataSet dp = new DataSet();
        da.Fill(dp);
        return dp;

    }

    [WebMethod]

    public string respuesta(Int32 resp, Int32 punta)
    {
        connection = new OracleConnection(ConnectionString);
        connection.Open();


        string sqlInsert = "insert into encuesta.RESPUESA (ALTERNATIVA,TBL_OPCIONES_EV_ID)";
        sqlInsert += "values (:resp,:punta)";

        OracleCommand cmdInsert = new OracleCommand();
        cmdInsert.CommandText = sqlInsert;
        cmdInsert.Connection = connection;



        OracleParameter respuesta = new OracleParameter();
        respuesta.Value = resp;
        respuesta.ParameterName = "resp";

        OracleParameter puntaje = new OracleParameter();
        puntaje.Value = punta;
        puntaje.ParameterName = "punta";


        cmdInsert.Parameters.Add(respuesta);
        cmdInsert.Parameters.Add(puntaje);

        cmdInsert.ExecuteNonQuery();

        cmdInsert.Dispose();

        return "ingreso correcto";
    }

    [WebMethod]

    public string respuestajefe(Int32 resp, Int32 punta)
    {
        connection = new OracleConnection(ConnectionString);
        connection.Open();


        string sqlInsert = "insert into encuesta.RESPUESA (ALTERNATIVA,TBL_OPCIONES_EV_ID)";
        sqlInsert += "values (:resp,:punta)";

        OracleCommand cmdInsert = new OracleCommand();
        cmdInsert.CommandText = sqlInsert;
        cmdInsert.Connection = connection;



        OracleParameter respuesta = new OracleParameter();
        respuesta.Value = resp;
        respuesta.ParameterName = "resp";

        OracleParameter puntaje = new OracleParameter();
        puntaje.Value = punta;
        puntaje.ParameterName = "punta";


        cmdInsert.Parameters.Add(respuesta);
        cmdInsert.Parameters.Add(puntaje);

        cmdInsert.ExecuteNonQuery();

        cmdInsert.Dispose();

        return "ingreso correcto";
    }


    [WebMethod]

    public string auditoriaIP(string ip, string fecha, string rut)
    {
        connection = new OracleConnection(ConnectionString);
        connection.Open();


        string sqlInsert = "insert into encuesta.TBL_AUDITORIA (DIRECCION_IP,FECHA_INGRESO,RUT_EVALUADO)";
        sqlInsert += "values (:ip,:fecha,:rut)";

        OracleCommand cmdInsert = new OracleCommand();
        cmdInsert.CommandText = sqlInsert;
        cmdInsert.Connection = connection;



        OracleParameter ipe = new OracleParameter();
        ipe.Value = ip;
        ipe.ParameterName = "ip";

        OracleParameter fechas = new OracleParameter();
        fechas.Value = fecha;
        fechas.ParameterName = "fecha";

        OracleParameter ruts = new OracleParameter();
        ruts.Value = rut;
        ruts.ParameterName = "rut";

        cmdInsert.Parameters.Add(ipe);
        cmdInsert.Parameters.Add(fechas);
        cmdInsert.Parameters.Add(ruts);

        cmdInsert.ExecuteNonQuery();

        cmdInsert.Dispose();

        return "ingreso correcto";
    }

    [WebMethod]
    public DataSet rutEvaluado()
    {
        connection = new OracleConnection(ConnectionString);
        connection.Open();
        OracleDataAdapter da = new OracleDataAdapter("select RUT,JEFE,TBL_AREA_INVESTIGACION_ID from encuesta.TBL_FUNCIONARIO", connection);
        DataSet dp = new DataSet();
        da.Fill(dp);
        return dp;

    }
