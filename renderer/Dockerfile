FROM continuumio/miniconda3  
  
RUN apt-get update && apt-get -y install unzip build-essential nodejs npm dumb-init
RUN npm install -g svgo

WORKDIR /  
RUN wget -O /tmp/ne_110m_land.zip http://naciscdn.org/naturalearth/110m/physical/ne_110m_land.zip  
WORKDIR /.local/share/cartopy/shapefiles/natural_earth/physical  
RUN unzip /tmp/ne_110m_land.zip && rm /tmp/ne_110m_land.zip  
  
RUN conda create -y -q -n my_cartopy_env -c conda-forge python=3.7 matplotlib cartopy proj4=5.2.0 pandas
  
ENV PATH /opt/conda/envs/my_cartopy_env/bin:$PATH  
  
RUN echo "conda activate my_cartopy_env" >> ~/.bashrc  
  
COPY requirements.txt ./
RUN pip install -r requirements.txt

WORKDIR /  
COPY app /app  

ENTRYPOINT ["/usr/bin/dumb-init", "--verbose", "--"]
CMD python3 /app/renderer.py