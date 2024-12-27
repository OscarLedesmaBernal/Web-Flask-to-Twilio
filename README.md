# Web-Flask-to-Twilio
Web Flask to Twilio
# Configuración para PythonAnywhere
        def configurar_pythonanywhere():
            try:
                # Configurar logging específico para PythonAnywhere
                logging.basicConfig(
                    filename='/home/tuusuario/logs/aplicacion.log',
                    level=logging.INFO,
                    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
                )
                
                # Configurar directorio de trabajo
                import os
                os.chdir('/home/tuusuario/miapp')
                
                # Configurar variables de entorno
                os.environ['OPENAI_API_KEY'] = 'tu-api-key'
                os.environ['TWILIO_ACCOUNT_SID'] = 'tu-account-sid' 
                os.environ['TWILIO_AUTH_TOKEN'] = 'tu-auth-token'
                
                logger.info("Configuración de PythonAnywhere completada")
                
            except Exception as e:
                logger.error(f"Error configurando PythonAnywhere: {e}")
                raise

        # Punto de entrada para WSGI
        def application(environ, start_response):
            """Función WSGI para PythonAnywhere"""
            try:
                # Configurar la aplicación
                configurar_pythonanywhere()
                
                # Crear instancia de Flask 
                return app(environ, start_response)
                
            except Exception as e:
                logger.error(f"Error en aplicación WSGI: {e}")
                status = '500 Internal Server Error'
                response_headers = [('Content-type', 'text/plain')]
                start_response(status, response_headers)
                return [b'Error interno del servidor']

        # Archivo de configuración WSGI
        """
        # Guardar como /var/www/tuusuario_pythonanywhere_com_wsgi.py:

        import sys
        path = '/home/tuusuario/miapp'
        if path not in sys.path:
            sys.path.append(path)
            
        from aplicacion import application
        """
